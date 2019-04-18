#
//
// @a1mtarabichi 
// -crypt0w1zmT
// 
// "Cryptow1z's Hit & Run T-Line Crystal Ball"
// 
// Use this indicator (using most accurate figures/variables) as a more dependable indicator to determine Squeeze/Fixed Momentum/Average True Range. This code was written as a variation of LazyBear's (shoutout to LazyBear, thanks for help) &  his formula was right range/variables & measurements needed to be fixed. It's not that they're inaccurate; but IMO they only account for EMA20/50 swing traders & so a combination of LazyBear's Squeeze Momentum Indicator & courtesy of Rick Saddler's TTR candlestick hit & run TP strategy: with KC (Keltner Channels), RSI (Relative Strength Index), BB (Bollinger's Bands), ATR(Average True Range), TTM(Squeeze), SMA / MACD (Simple Moving Average + Divergence), as well as range while accounting for volatility. 
// Several variations already exist but I've found LazyBear's to be the most precise based on my extensive experience trading (staring at charts until my eyes would bleed) to alter variables to ones that are much more accurate than any other indictaor (I've seen atleast) courtesy of Rick Saddler's T-Line hit & run EMA8,EMA12,EMA15 so I could have a reliable TP indicator which pivots on timeframes for all Forex traders and not one broad type i.e. 20-day moving average swing traders etc. so in a sense this source code adds additional commands to create something that is more in line with actual market trajectory.  
// This source code will benefit MOST swing traders & is applicable to any timeframe on each chart (WITH THE EXCEPTION OF 1MIN, 3MIN & 5MIN) as for 15min, I would generally avoid that as well (not to say its inaccurate) but so far testing it out has yielded accurate results on  30min, 45min, 1H, 2H, 3H, 4H, 6H, 1D charting.
// Perfect flexibility for any swing-trader; blanket trading strategy for ALL forex swing-traders. unless you're  day trading or a HODLER (I mean a real HODLER, not those idiots who bag hold for 1Q & think they are long-term investors) than this is 95% reliable (IMO) and open, free to public for use. 
// I encourage everyone & anyone to use it & provide feedback!  PLEASE do be BRUTALLY honest. ESPECIALLY seasoned traders who are MUCH better than I am (of which there are MANY), test this out & give me your 2 cents. Only feedback can lead to progress; I've made what I thought is best thus far but for the OGs (trading since as far bask as the great depression) I would LOVE your constructive criticism.
// Those familiar with hit & run candlestick trading will benefit most from this indicator. Shoutout to LAZYBEAR & RICK SADDLER for essentially using Bear's formula & Saddler's strategy & combining them to create what I call:
//
//  "Cryptow1z's Hit & Run T-Line Crystal Ball"
//
study(shorttitle = "CBALL_WIZ", title="Cryptowiz's Crystall Ball T-Line CC HitnRun' / SQZ_MOM [a1mtarabichi]", overlay=false)

length = input(27, title="BBL")
mult = input(5.5,title="BBMF")
lengthKC=input(21, title="KCL")
multKC = input(2.8, title="KCMF")

useTrueRange = input(true, title="Use TrueRange (KC)", type=bool)

// Calculate BB
source = close
basis = sma(source, length)
dev = multKC * stdev(source, length)
upperBB = basis + dev
lowerBB = basis - dev

// RSI
RSIlength = input(6,title="RSI Period Length") 
RSIoverSold = 50
RSIoverBought = 50
price = close
vrsi = rsi(price, RSIlength)



// Calculate KC
ma = sma(source, lengthKC)
range = useTrueRange ? tr : (high - low)
rangema = sma(range, lengthKC)
upperKC = ma + rangema * multKC
lowerKC = ma - rangema * multKC

sqzOn  = (lowerBB > lowerKC) and (upperBB < upperKC)
sqzOff = (lowerBB < lowerKC) and (upperBB > upperKC)
noSqz  = (sqzOn == false) and (sqzOff == false)

val = linreg(source-avg(avg(highest(high,lengthKC), lowest(low, lengthKC)),sma(close,lengthKC)), 
            lengthKC,0)

bcolor = iff( val > 0, 
            iff( val > nz(val[1]), lime, green),
            iff( val < nz(val[1]), red, maroon))
scolor = noSqz ? yellow : sqzOn ? yellow : black 
plot(val, color=bcolor, style=area, linewidth=1)
plot(0, color=scolor, style=cross, linewidth=1)




