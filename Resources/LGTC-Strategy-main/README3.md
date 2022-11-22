# Timing-Strategy
Crypto dribblings
// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © Leigh_Goullet

//@version=5
strategy("My strategy", overlay=true)
strategy("Strat with time delay", overlay = true)

timeUnitsQty = -input.int(20, "Quantity", inline="Delay", minval=0, tooltip="Use 0 for no delay")
timeUnitType = input.string("minutes", "", inline="Delay", options=["seconds", "minutes", "hours", "days", "months", "years"])

// ————— Converts current chart timeframe into a float minutes value.
tfInMinutes() =>
    tfInMinutes = timeframe.multiplier * (timeframe.isseconds ? 1. / 60 : timeframe.isminutes ? 1. : timeframe.isdaily ? 60. * 24 : timeframe.isweekly ? 60. * 24 * 7 : timeframe.ismonthly ? 60. * 24 * 30.4375 : na)

// ————— Calculates a +/- time offset in variable units from the current bar"s time or from the current time.
// WARNING:
//      This functions does not solve the challenge of taking into account irregular gaps between bars when calculating time offsets.
//      Optimal behavior occurs when there are no missing bars at the chart resolution between the current bar and the calculated time for the offset.
//      Holidays, no-trade periods or other irregularities causing missing bars will produce unpredictable results.
timeFrom(from, qty, units) =>
    // from  : starting time from where the offset is calculated: "bar" to start from the bar"s starting time, "close" to start from the bar"s closing time, "now" to start from the current time.
    // qty   : the +/- qty of _units of offset required. A "series float" can be used but it will be cast to a "series int".
    // units : string containing one of the seven allowed time units: "chart" (chart"s resolution), "seconds", "minutes", "hours", "days", "months", "years".
    int timeFrom = na
    // Remove any "s" letter in the _units argument, so we don"t need to compare singular and plural unit names.
    unit = str.replace_all(units, "s", "")
    // Determine if we will calculate offset from the bar"s time or from current time.
    t = from == "bar" ? time : from == "close" ? time_close : timenow
    // Calculate time at offset.
    if units == "chart"
        // Offset in chart res multiples.
        timeFrom := int(t + tfInMinutes() * 60 * 1000 * qty)
    else
        // Add the required qty of time units to the from starting time.
        y = year(t) + (unit == "year" ? int(qty) : 0)
        m = month(t) + (unit == "month" ? int(qty) : 0)
        d = dayofmonth(t) + (unit == "day" ? int(qty) : 0)
        h = hour(t) + (unit == "hour" ? int(qty) : 0)
        min = minute(t) + (unit == "minute" ? int(qty) : 0)
        s = second(t) + (unit == "econd" ? int(qty) : 0)
        // Return the resulting time in ms Unix time format.
        timeFrom := timestamp(y, m, d, h, min, s)

// Entry conditions.
ma = ta.sma(close, 100)
goLong = close > ma
goShort = close < ma

// Time delay filter
var float lastTradeTime = na
if nz(ta.change(strategy.position_size), time) != 0
    // An order has been executed; save the bar"s time.
    lastTradeTime := time
    lastTradeTime
// If user has chosen to do so, wait `timeUnitsQty` `timeUnitType` between orders
delayElapsed = timeframe.period()

if goLong and delayElapsed
    strategy.entry("Long", strategy.long, comment="Long")
if goShort and delayElapsed
    strategy.entry("Short", strategy.short, comment="Short")

plot(ma, "MA", goLong ? color.lime : color.red)
plotchar(delayElapsed, "delayElapsed", "•", location.top, size=size.tiny)


var begin = time
days = (time - begin) / (24 * 60 * 60 * 1000)
plot(days)
print(text) =>
    var lbl = label.new(bar_index, na, text, xloc.bar_index, yloc.price, color(na), label.style_label_up, color.gray, size.large, text.align_left)
    label.set_xy(lbl, bar_index, days)
    label.set_text(lbl, text)

if barstate.islast
    print(str.tostring(days, "#.0 days\n") + str.tostring(bar_index + 1, "# bars"))