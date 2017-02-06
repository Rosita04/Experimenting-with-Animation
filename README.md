# Experimenting-with-Animation
<!DOCTYPE html>


<body bgcolor="fffae3">
<meta charset="utf-8">
<link href="https://fonts.googleapis.com/css?family=Dosis|Raleway" rel="stylesheet">
<style type="text/css">



svg {
    font-family: 'Dosis', sans-serif;
}

.line {
    fill: none;
    stroke: #000;
    stroke-width: 4px;
}
</style>


<body>



<div class= "resume">

<center><img src = "images/resume2.jpg"></center>


</div>


<div class="main">

<button onclick="toPie()" >Click to Pie</button>
<button onclick="toBar()" >Click to Bar</button>
    <script src="https://d3js.org/d3.v3.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script>
    <script>
    var m = [20, 20, 30, 20],
        w = 960 - m[1] - m[3],
        h = 450 - m[0] - m[2];

    var x,
        y,
        duration = 3000,
        delay = 400;

    var color = d3.scale.category10();

    var svg = d3.select("body").append("svg")
        .attr("width", w + m[1] + m[3])
        .attr("height", h + m[0] + m[2])
        .append("g")
        .attr("transform", "translate(" + m[3] + "," + m[0] + ")");

    var data = [{
        key: "HTML & CSS",
        maxPrice: 223.02,
        sumPrice: 7961.850000000001
    }, {
        key: "PHP",
        maxPrice: 135.91,
        sumPrice: 5902.409999999999
    }, {
        key:"Creative Cloud",
        maxPrice: 130.32,
        sumPrice: 11225.13
    }, {
        key: "Javascript",
        maxPrice: 43.22,
        sumPrice: 3042.6200000000017
    }]

    x = d3.scale.ordinal();

    y = d3.scale.linear();

    x.domain(data.map(function(d) {
            return d.key;
        }))
        .rangeRoundBands([0, w], .2);

    var pie = d3.layout.pie()
        .value(function(d) {
            return d.sumPrice;
        });

    var arc = d3.svg.arc();

    y.domain([0, d3.max(data.map(function(d) {
            return d.sumPrice;
        }))])
        .range([h / 4 - 20, 0]);

    var g = svg.selectAll(".symbol")
        .data(function() {
            return pie(data);
        })
        .enter()
        .append("g")
        .attr("class", "symbol");

     g.append("rect")
        .style("fill", function(d) {
            return color(d.data.key);
        })
        .attr("x", function(d){
            return x(d.data.key);
        })
        .attr("y", function(d){
            return y(d.data.sumPrice);
        })
        .attr("width", x.rangeBand())
        .attr("height", function(d) {
                return h - y(d.data.sumPrice);
            });

        
        g.append("path")
        .style("fill", function(d) {
            return color(d.data.key);
        });

        g.append("text")
        .attr("transform", function(d){
            return "translate(" + (x(d.data.key)+x.rangeBand()/3) + "," + (h+20) + ")";
        })
        .text(function(d) {
            return d.data.key;
        });
    //then use path element of bars do transition;

function toPie(){
    var g = svg.selectAll(".symbol");

    g.selectAll("rect").remove();

    g.selectAll("path")
        .transition()
        .duration(duration)
        .tween("arc", arcTween);
                                    
    function arcTween(d) {
        var path = d3.select(this),
            text = d3.select(this.parentNode).select("text"),
            x0 = x(d.data.key),
            y0 = h - y(d.data.sumPrice); //initial height

        return function(t) {
            var r = h / 2 / Math.min(1, t + 1e-3),
                //a is stepping factor, starting from 1 to 0,
                //as the timer t goes.
                //A simper alternative: a = 1 - t;
                a = Math.cos(t * Math.PI / 2),
                xx = (-r + (a) * (x0 + x.rangeBand()) + (1 - a) * (w + h) / 2),
                yy = ((a) * h + (1 - a) * h / 2),
                f = {
                    innerRadius: (r - x.rangeBand() / (2 - a)) * a,
                    //pie chart: (r - x.rangeBand() / (2 - a)) * a,
                    outerRadius: r,
                    startAngle: a * (Math.PI / 2 - y0 / r) + (1 - a) * d.startAngle,
                    endAngle: a * (Math.PI / 2) + (1 - a) * d.endAngle
                };


            path.attr("transform", "translate(" + xx + "," + yy + ")");
            path.attr("d", arc(f));
            text.attr("transform", "translate(" + arc.centroid(f) + ")translate(" + xx + "," + yy + ")rotate(" + ((f.startAngle + f.endAngle) / 2 + 3 * Math.PI / 2) * 180 / Math.PI + ")");
        };
    }
}

function toBar(){
    var g = svg.selectAll(".symbol");

    g.selectAll("path")
        .transition()
        .duration(duration)
        .tween("arc", arcTween);

                                       
    function arcTween(d) {
        var path = d3.select(this),
            text = d3.select(this.parentNode).select("text"),
            x0 = x(d.data.key),
            y0 = h - y(d.data.sumPrice); //initial height

        return function(t) {
            t = 1-t;
            var r = h / 2 / Math.min(1, t + 1e-3),
                //a is stepping factor, starting from 1 to 0,
                //as the timer t goes.
                //A simper alternative: a = 1 - t;
                a = Math.cos(t * Math.PI / 2),
                xx = (-r + (a) * (x0 + x.rangeBand()) + (1 - a) * (w + h) / 2),
                yy = ((a) * h + (1 - a) * h / 2),
                f = {
                    innerRadius: (r - x.rangeBand() / (2 - a)) * a,
                    //pie chart: (r - x.rangeBand() / (2 - a)) * a,
                    outerRadius: r,
                    startAngle: a * (Math.PI / 2 - y0 / r) + (1 - a) * d.startAngle,
                    endAngle: a * (Math.PI / 2) + (1 - a) * d.endAngle
                };


            path.attr("transform", "translate(" + xx + "," + yy + ")");
            path.attr("d", arc(f));
            text.attr("transform", "translate(" + arc.centroid(f) + ")translate(" + xx + "," + yy + ")rotate(" + ((f.startAngle + f.endAngle) / 2 + 3 * Math.PI / 2) * 180 / Math.PI + ")");
        };
    }
}     

    </script>

     </div>
<html>
