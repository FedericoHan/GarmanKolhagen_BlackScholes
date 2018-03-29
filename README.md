# GarmanKolhagen_BlackScholes
FX _Currency vanilla pricer

Lots of formulas on the web or in books but few actual implementations, so here is one application implemented as a Python class.

I've included a bit of commentary on the various conventions from the currency option market, ie, delta is not in units of 
"shares" but in milions of the "Foreign" currency. What is the "foreign" currency?
The FX market refers to:
  'USDJPY" = 112.24...means 112.24JPY buy 1 USD. In this case, JPYen is the domestic unit, USD the foreign.  
  "AUDUSD" = 0.7725 ...means 0.7725USD buy 1 AUD.  In this case, USD is the domestic unit, AUD the foreign.  

The __init__ method includes option specific attributes like strike and tenor.

Market inputs like spot, volatility and interest rates are taken as arguments for pricing methods. 

delta() method has, as per above, various conventions. For AUDUSD and EURUSD it's excluding premium if the premium is paid in USD.
For USDXYZ you exclude the premium as it's often quoted in USD (as well as AUDUSD and EURUSD if premium is in %Foreign).

gamma() method is numerical (bump up and down delta)

strike_From_delta() method , ask if cares. There is a slight circularity here btw.

CND() method is a (very close) approximation of NORMSDIST.

Almost all results are pretty close to what actual pricers yield. 2 examples below from the Python Notebook, as of 24Nov2017.

Example 1: USD50mio , put USD/CNH call expiring 12April2018 (2weeks), struck at 6.27. 
Foreign (USD) rate is 1.7068%, 
domestic (CNH) rate is 3.3692%

Premium is in % of Foreign (USD) in this case, while delta is SIF (Spot, Including Premium, in Foreign Units).

opt_put = FXOption(CallPutFlag = 'Put', K = 6.27, dateEnd = date(2018,4,12), 
                notional = 50000000, 
                premConvention = "")

S = 6.2859
sigma  = 0.0596
rFor = 0.017068
rDom = 0.033692

print('value: '+'{:,}'.format(opt_put.price(S , sigma , rFor, rDom)))  
print('delta: '+'{:,}'.format(opt_put.delta( S  , sigma , rFor, rDom, convention = 'SIF' )))
print('gamma: '+'{:,}'.format(opt_put.gamma(S, sigma , rFor, rDom, convention = 'SEF' )))


Example 2: EUR 100mio call 1.2330 EUR/USD put exiring 5 Aprl 2018 (1week).
Here Foreign rate is EUR, domestic is USD, as it's USD 1.2310 for 1 EUR.  

S = 1.2310
sigma = 0.0625
rFor = -0.00755
rDom = 0.0172138

print('{:,}'.format(eur_SEF.price(S, sigma , rFor , rDom )))
print('{:,}'.format(eur_SEF.delta( S, sigma , rFor , rDom, convention = 'SEF' )))
print('{:,}'.format(eur_SEF.gamma( S, sigma , rFor , rDom, convention = 'SEF' )))
