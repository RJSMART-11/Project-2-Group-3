# LGTC-Strategy
Crypto dribblings
//@version=5
// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © Leigh_Goullet

indicator(title='EMA/SL', overlay=true)

Bars = input.int(title='# bars left & right of pivot', defval=2, minval=1)
fast_ema = input.int(title='Fast EMA period', defval=10, minval=1)
slow_ema = input.int(title='Slow EMA period', defval=20, minval=1)
bbmulti = input.float(title='Bull/Bear candle percent', defval=0.6, minval=0.5, maxval=0.99)

// DOUBLE EMA --------------------------------------------------
emaF = ta.ema(close, fast_ema)
emaS = ta.ema(close, slow_ema)

emaFplot = plot(emaF, color=color.new(color.green, 0), style=plot.style_line, linewidth=2, title='EMA Fast')
emaSplot = plot(emaS, color=color.new(color.red, 0), style=plot.style_line, linewidth=2, title='EMA Slow')
fill(emaFplot, emaSplot, color=emaF > emaS ? color.green : color.red, editable=false, transp=85)

//Bullish/Bearish Candle
plotchar(close, title='Bull/Bear Candle', color=close > low + (high - low) * bbmulti ? color.green : close < high - (high - low) * bbmulti ? color.red : color.yellow, char='▴', location=location.belowbar, show_last=2)
//barcolor(color=close>low+((high-low)*bbmulti)? green:close<high-((high-low)*bbmulti)? red:yellow, show_last=2)

// STOP LOSS (match with zigzag until pinescript allows better scripting)-----------------------
H = ta.pivothigh(Bars, Bars)
L = ta.pivotlow(Bars, Bars)

// Long stop inputs
prevL = ta.valuewhen(L, L, 0)
prevL1 = ta.valuewhen(L, L, 1)
confirmprevL = ta.pivothigh(Bars, 0)

// Short stop inputs
prevH = ta.valuewhen(H, H, 0)
prevH1 = ta.valuewhen(H, H, 1)
confirmprevH = ta.pivotlow(Bars, 0)

// Trend analysis
emaUp = emaS < emaF
emaDn = emaS > emaF
//trendUp         = prevH1<prevH and prevL1<prevL and emaUp? 1:0
//trendDn         = prevH1>prevH and prevL1>prevL and emaDn? 1:0

// Determine and plot stop loss
stopL = (emaDn and confirmprevH > 0 and close[1] < hl2[1] and low < low[1] and low[1] < low[2] and high < prevH[1])[1] ? prevH[1] : (emaUp and confirmprevL > 0 and close[1] > hl2[1] and high > high[1] and high[1] > high[2] and low > prevL[1])[1] ? prevL[1] : na
plotchar(stopL, title='Stop loss', color=color.rgb(243, 9, 9), char='×', location=location.absolute, show_last=7, size=size.tiny, offset=-1)
