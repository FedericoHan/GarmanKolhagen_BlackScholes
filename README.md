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

Example 1: USD100mio,  put USD/JPY call expiring 26Jun2018 (7months), struck at 101.45. Foreign (USD) rate is 1.458%, domestic (JPY)
rate is -0.603, implied vol is 10.51%.  Premium is in % of Foreign (USD) in this case, while delta is SIF (Spot, Including Premium, in Foreign Units).

opt1 = FXOption('Put', 111.45, 101.45, 0.1051,  0.01458, -0.00603, date(2018,6, 26), 100000000,"")
print(opt1.price())     ----> USD 616,169
print(opt1.delta('SIF')) ----> USD 14.989mio 
print(opt1.gamma())   ----> USD 2,5mio (this is a bit off)
print(opt1.vega())  -----> USD 173,000


Example 2: EUR 52mio,  call EUR/USD put, expiring 24Aug2018 (7months), struck at 1.3200. Foreign (EUR) rate is -0.82%, domestic (USD in this case) rate is 1.50$, implied vol is 8.36%.  Premium in this case is in domestic (USD) units, delta is SEF (Spot, Excluding Premium in Foreign Units).

opt2 = FXOption('Call', 1.1850, 1.3200, 0.0836,  -0.00824, 0.01504,date(2018, 8, 24), 52000000, 'DomesticPips')
print(opt2.price())    ----> USD 236,461
print(opt2.delta('SEF')) ---> EUR 5.8mio
print(opt2.gamma())  ---> EUR .113mio, way wrong
print(opt2.vega())  --->EUR 86k
