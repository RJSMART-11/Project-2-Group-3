# HH_LL_ZZ-Strategy
Crypto dribblings
// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © fikira
//@version=5
indicator("HH-LL ZZ", shorttitle='H²L²Z²', max_lines_count=500, max_labels_count=500, max_bars_back=1000, overlay=true)

cl      = input.string(     'close'    , 'source for level breach', options=['H/L', 'close']                                        )
cnt     = input.int   (        3       , 'x breaches'             , minval=1, maxval=10                                             )
style   = input.string(line.style_solid, 'line style'             , options=[line.style_solid, line.style_dotted, line.style_dashed])
showLev = input.bool  (      false     , 'show levels breaches'                                                                     )
showS_R = input.bool  (      false     , 'show Support/Resistance'                                                                  )
ds      = input.bool  (      false     , 'remove repaint warning'                                                                   )
lab     = input.bool  (      false     , 'labels'                                                                                   )
lin     = input.bool  (      true      , 'lines'                                                                                    )

var line [] lines   = array.new<line> ()
var label[] labels  = array.new<label>()
var float   levelH  = high
var float   levelL  = low
var int     countLL = 0
var int     countHH = 0
var int     dir     = 0
bool        h       = false 
bool        l       = false 

getSet(l, i) =>
    if array.size(labels) > i -1
        getX = label.get_x(array.get(labels, i)), getY = label.get_y(array.get(labels, i))
        line.set_xy1(l, getX, getY), line.set_xy2(l, getX +1, getY)
        
if array.size(lines) > 500  
    line.delete(array.pop(lines))
if array.size(labels) > 500
    label.delete(array.pop(labels))

if barstate.isfirst 
    array.unshift(labels, label.new(bar_index, hl2))

if (cl == 'H/L' ? low : close) < levelL 
    countLL += 1
    if countLL  ==  cnt
        l       :=  true
        countHH :=  0
        countLL :=  0
        levelL  :=  low
        levelH  :=  high    
        if dir   > -1
            array.unshift(labels, label.new(bar_index, low, style=label.style_label_up, color=lab ? #FF0000 : color.new(color.blue, 100)))
            if array.size(labels) > 1
                array.unshift(lines , line.new (label.get_x(array.get(labels, 1)), label.get_y(array.get(labels, 1)), bar_index, low, color=lin ? #FF0000 : color.new(color.blue, 100), style=style))
            dir := -1
            if array.size(labels) > 2
                label.set_color(array.get(labels, 2), lab ? color.yellow : color.new(color.blue, 100))
            if array.size(lines) > 2
                line.set_color (array.get(lines , 2), lin ? color.yellow : color.new(color.blue, 100))
        else
            label.set_xy(array.get(labels, 0), bar_index, low)
            line.set_xy2(array.get(lines , 0), bar_index, low)
        if array.size(labels) > 2     
            hi = low
            bx = 0
            for i =  0 to bar_index - label.get_x(array.get(labels, 2)) 
                if high[i] > hi
                    hi := high[i]
                    bx := bar_index - i
            label.set_xy(array.get(labels, 1), bx, hi)
            line.set_xy2(array.get(lines , 1), bx, hi)
            line.set_xy1(array.get(lines , 0), bx, hi)

if (cl == 'H/L' ? high : close) > levelH 
    countHH += 1
    if countHH  ==  cnt
        h       :=  true
        countHH :=  0
        countLL :=  0
        levelL  :=  low
        levelH  :=  high
        if dir   <  1  
            array.unshift(labels, label.new(bar_index, high, style=label.style_label_down, color=lab ? #FF0000 : color.new(color.blue, 100)))
            if array.size(labels) > 1
                array.unshift(lines , line.new (label.get_x(array.get(labels, 1)), label.get_y(array.get(labels, 1)), bar_index, high, color=lin ? #FF0000 : color.new(color.blue, 100), style=style))
            dir :=  1
            if array.size(labels) > 2
                label.set_color(array.get(labels, 2), lab ? color.yellow : color.new(color.blue, 100))
            if array.size(lines) > 2
                line.set_color (array.get(lines , 2), lin ? color.yellow : color.new(color.blue, 100))
        else
            label.set_xy(array.get(labels, 0), bar_index, high)
            line.set_xy2(array.get(lines , 0), bar_index, high)
        if array.size(labels) > 2   
            lo = high
            bx = 0  
            for i =  0 to bar_index - label.get_x(array.get(labels, 2)) 
                if low [i] < lo
                    lo := low[i]
                    bx := bar_index - i
            label.set_xy(array.get(labels, 1), bx, lo)
            line.set_xy2(array.get(lines , 1), bx, lo)
            line.set_xy1(array.get(lines , 0), bx, lo)

if barstate.islastconfirmedhistory and not ds
	var tab = table.new(position = position.top_right, columns = 1, rows = 1, bgcolor = color.new(color.blue, 75), border_width = 1)
	table.cell(table_id = tab, column = 0, row = 0, text = "Red labels and lines could possibly repaint!", text_color= #FF0000, text_size = size.small, text_font_family = font.family_monospace)

plotshape(showLev and h, style=shape.circle, location=location.abovebar, color=color.lime, size=size.tiny, display=display.pane)
plotshape(showLev and l, style=shape.circle, location=location.belowbar, color=color.blue, size=size.tiny, display=display.pane)

var line l0 = line.new(na, na, na, na, extend=extend.right, style=line.style_dotted, color=color.new(#FF0000   , 25))
var line l1 = line.new(na, na, na, na, extend=extend.right, style=line.style_dotted, color=color.new(#FF0000   , 25))
var line l2 = line.new(na, na, na, na, extend=extend.right, style=line.style_dotted, color=color.new(color.blue,  0))
var line l3 = line.new(na, na, na, na, extend=extend.right, style=line.style_dotted, color=color.new(color.blue,  0))

if barstate.islast and showS_R 
    getSet(l0, 0), getSet(l1, 1)
    getSet(l2, 2), getSet(l3, 3)
       