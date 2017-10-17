title: 关于
---
# 关于我

1980年出生于现在高考的日子,在汉中这个经常在史书上提到的兵家必争之地长大，小的时候那是一个小胖子
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS_V2BabIYmklHd4PH8Wy9B1mfboom-EWwMcMTINP44jFpVm54_EA" alt="6year" style="width:60px;height:60px;">

不知道从什么时候开始我特别着迷足球，每天踢每天踢，高中时候又一次从下午一直踢到天黑，也不觉得累，体重不到120

毕业后就是繁重的工作，我换过好几次工作，因为各种原因，接触到很多项目，人，有感激的，也有讨厌的，



# 大学生活

我毕业于西北工业大学，计算机科学与工程专业

毕业后的几年，为了充实自己，我又读了 上海交大软件学院的在职硕士



# 工作这些年

sinobull
ebest
ebay 
hp


# 旅游 

我喜欢旅游，喜欢去很多城市转转，吃吃当地美食

# 我的作品

有趣的项目

# 未来

其实做程序员是一件蛮悲哀的事情，因为很多公司会觉得你的年龄和你的薪资不在匹配，如果没有更高级的头衔，我觉得




我的体重就和这个图标一样，不断变化
<script src="https://code.jquery.com/jquery-2.2.3.min.js"></script>
<script src="https://d3js.org/d3.v3.min.js" charset="utf-8"></script>
<script type="text/javascript" src="http://xuchen-81.appspot.com/js/dj.min.js"></script>

   <div id="canvas"></div>


<!--    <style> /* set the CSS */

body { font: 12px Arial;}

path { 
    stroke: steelblue;
    stroke-width: 2;
    fill: none;
}

.axis path,
.axis line {
    fill: none;
    stroke: grey;
    stroke-width: 1;
    shape-rendering: crispEdges;
}

</style>
<!-- load the d3.js library -->    
<script src="http://d3js.org/d3.v3.min.js"></script>

<script>

// Set the dimensions of the canvas / graph
var margin = {top: 30, right: 20, bottom: 30, left: 50},
    width = 600 - margin.left - margin.right,
    height = 270 - margin.top - margin.bottom;

// Parse the date / time
var parseDate = d3.time.format("%d-%b-%y").parse;

// Set the ranges
var x = d3.time.scale().range([0, width]);
var y = d3.scale.linear().range([height, 0]);

// Define the axes
var xAxis = d3.svg.axis().scale(x)
    .orient("bottom").ticks(5);

var yAxis = d3.svg.axis().scale(y)
    .orient("left").ticks(5);

// Define the line
var valueline = d3.svg.line()
    .x(function(d) { return x(d.date); })
    .y(function(d) { return y(d.close); });
    
// Adds the svg canvas
var svg = d3.select("body")
    .append("svg")
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
    .append("g")
        .attr("transform", 
              "translate(" + margin.left + "," + margin.top + ")");

// Get the data
data = [
{date:"7-June-1980",close:8.13},
{date:"1-Apr-1983",close:30.98},
{date:"27-Apr-1986",close:67.00},
{date:"26-Apr-1998",close:60.70},
{date:"25-Apr-2002",close:75.00},
{date:"24-Apr-2006",close:60.28},
];

//d3.csv("data.csv", function(error, data) {
   data.forEach(function(d) {
        d.date = parseDate(d.date);
        d.close = +d.close;
    });

    // Scale the range of the data
    x.domain(d3.extent(data, function(d) { return d.date; }));
    y.domain([0, d3.max(data, function(d) { return d.close; })]);

    // Add the valueline path.
    svg.append("path")
        .attr("class", "line")
        .attr("d", valueline(data));

    // Add the X Axis
    svg.append("g")
        .attr("class", "x axis")
        .attr("transform", "translate(0," + height + ")")
        .call(xAxis);

    // Add the Y Axis
    svg.append("g")
        .attr("class", "y axis")
        .call(yAxis);

//});

</script> -->