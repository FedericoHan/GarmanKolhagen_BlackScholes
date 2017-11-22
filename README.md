# GarmanKolhagen_BlackScholes
FX _Currency vanilla pricer

Lots of formulas on the web or in books but few actual implementations, so here is one application implemented as a Python class.

I've included a bit of commentary on the various conventions from the currency option market, ie, delta is not in units of 
"shares" but in milions of the "Foreign" currency. What is the "foreign" currency?
The FX market refers to:
  "AUDUSD" = 0.7725 ...means 0.7725USD buy 1 AUD.  In this case, USD is the domestic unit, AUD the foreign.  
  'USDJPY" = 112.24...means 112.24JPY buy 1 USD. In this case, JPYen is the domestic unit, USD the foreign.  
  
  

The __init__ method includes all the usual inputs (spot, strike, implied vol blabla), as well as stuff like d1, d2 to price option and greeks.  
delta() method has, as per above, various conventions. For AUDUSD and EURUSD it's excluding premium. For USDXYZ you exclude the premium as it's often quoted in USD.

strike_From_delta() method , ask if cares. There is a slight circularity here btw.

CND() method is a (very close) approximation of NORMSDIST.

Almost all results are pretty close to what actual pricers yield (except gamma, need to check if a typo there).


 
