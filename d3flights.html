
<!DOCTYPE html>
<html lang="en">
 <head>
  <meta charset="utf-8" />
  <title>d3.js airport data from Google Fusion - ramblings.mcpher.com</title>

  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <link rel="stylesheet" type="text/css" href="http://xliberation.com/cdn/css/d3flights.css">
  <script type="text/javascript" src="http://d3js.org/d3.v2.min.js"></script>
  <script type="text/javascript" src="https://www.google.com/jsapi"></script>
  <script type="text/javascript">
   google.load("jquery", "1");
   google.setOnLoadCallback(function() {
       var layoutTesting = false;
       makeLight("d3","busy");
       if (!layoutTesting)
       initialize().done (
            function (control) {
                doTheViz(control);
                makeLight("d3","idle");
            }
        )
        .fail (function(error) {
            alert (JSON.stringify(error));
            makeLight("d3","failed");
        })
   });
  </script>



<script>

function doTheViz(control) {
    var svg = control.svg;
    var package = control.package;
    var options = control.options;
    var projection = control.projection;
    
    var path = d3.geo.path()
        .projection(projection);

    var states = svg.append("svg:g")
        .attr("id", "states");

    var arcs = svg.append("svg:g")
        .attr("id", "arcs");

    var circles = svg.append("svg:g")
        .attr("id", "circles");
    
    states.selectAll("path")
          .data(package.states.features)
        .enter().append("svg:path")
          .attr("d", path);

    // the paths between airports
    var arcsEnter = arcs.selectAll("g")
        .data(control.clean.routes)
        .enter();

    var arcPaths = arcsEnter.append("svg:path")
        .attr("class","arcpath")
        .attr("d", function(d) {
              var arc = d3.geo.greatArc()
                .source(package.knownAirports[d.origin].location)
                .target(package.knownAirports[d.dest].location );
            return path(arc(d)); 
        })
        .style("stroke", control.options.routeStroke)
        .style("stroke-width", control.options.routeStrokeWidth)
        .on("mouseover", function(d){
            
            var elem = d3.select(this);
            if (elem.style("display") != 'none') {
            // just consider visible routes 
                // hide the others
                arcPaths.style("display","none");
                // make selected double width for single route only
                arcPaths.filter(function(a) { 
                    return (a.origin == d.origin && a.dest == d.dest);
    
                })
                .style("display","inline")
                .style("stroke", control.options.routeFocusStroke)
                .style("stroke-width", control.options.routeFocusStrokeWidth*control.options.routeMagnify);
                
                // hide all cities not on this route
                
                circle.filter (function (a) {
                    return !(a.airport.iata == d.origin || a.airport.iata == d.dest);
                })
                .style("display","none");
                
                doDelayBars(createDelayBars(control,"carrier", {eitherWay: d }),{eitherWay:d});
            } 
        })
        .on("mouseout", function(d){
            
            var elem = d3.select(this);
            if (elem.style("display") != 'none') {
                elem.style("stroke", control.options.routeStroke)
                    .style("stroke-width", control.options.routeStrokeWidth);
                
                // bring them back
                showRoutes (clickCity());
                doDelayBars(createDelayBars(control,"carrier"));
            }
        });
     
    arcPaths.append("svg:title")
        .text( function(d) {
            var route = control.clean.knownRoutes[routeKey(d)];
            var s = [];
            var known = control.package.knownAirports;
            s.push(getAirportDetails (known[route.origin]) + "<-->");
            s.push(getAirportDetails (known[route.dest]));
            
            return s.join ("\n") + '\n' + describeRoute (route).join("\n") + '\n' +
                    describeRoute ({dest:route.origin,origin:route.dest}).join("\n");
        }); 
    


    // the airports
    var node = circles.selectAll("circle")
        .data(control.clean.airports, function(d) { 
            return d.airport.iata; });

    var nodeEnter = node.enter();
    
    var circle = nodeEnter.append("svg:circle")
        .style("fill", control.options.circleFill)
        .attr("cx", function(d, i) { return d.airport.position[0]; })
        .attr("cy", function(d, i) { return d.airport.position[1]; })
        .attr("r", function(d, i) { 
            var aResult = sumResults (control, {airport:d.airport.iata});
            return Math.sqrt(control.options.radius*aResult.totalFlights); 
        })
        .sort(function(a, b) { 
            var aResult = sumResults (control, {airport:a.airport.iata});
            var bResult = sumResults (control, {airport:b.airport.iata});
            return +bResult.totalFlights - aResult.totalFlights; 
        })

        .on("mouseover", function(d){
            cityMouseOver(d3.select(this) ,d);
        })
        
        .on("mouseout", function(d){
            
            var elem = d3.select(this);
            // just consider visible cities
            if (elem.style("display") != 'none') {
                resetCircle(elem);
                showRoutes (clickCity());
                doDelayBars(createDelayBars(control,"carrier"));
            }
        })
        
        .on("click", function(d){
            // click will freeze the current city
            var elem = d3.select(this);
            var city = clickCity();
            if (city && city.airport.iata == d.airport.iata) { 
                // unset current
                control.clicked = null;
                resetCircle(elem);
               
            }
            else { 
                // new click
                var newElem = d3.select(this);
                if (city) { 
                    var elem = control.clicked.elem;
                    control.clicked = null;
                    resetCircle(elem);
                }
                control.clicked = { type:'city', d: d , elem: newElem};
                resetCircle(newElem);
            }
            cityMouseOver(d3.select(this) ,d);
        });
        
    
    circle.append("svg:title")
        .text(function(d) { 
            var s = [];
            s.push(getAirportDetails (d.airport));
            
            return s.join ("\n") + "\nfrom " + 
                describeAirport (d.airport.iata,{originAirport:d.airport.iata}).join("\n") +
                "\nto " +
                describeAirport (d.airport.iata,{destAirport:d.airport.iata}).join("\n");

        });
    
    function resetCircle (elem) {
        
        var city = clickCity();
        elem.style("fill", function(d){ 
                if (city && city.airport.iata == d.airport.iata)  
                    return control.options.nodeClickFill ;
                else
                    return control.options.circleFill ;
            })
            .attr("r", function(d) { 
                if (city && city.airport.iata == d.airport.iata) 
                    return control.options.nodeFocusRadius;
                else {
                    var aResult = sumResults (control, {airport:d.airport.iata});
                    return Math.sqrt(control.options.radius*aResult.totalFlights);
                }
            });
    }
    function cityFilter(city,d) {
        
        // the matching routes
        arcPaths.filter(function(a) { 
            if (city)
                return (a.origin==city.airport.iata || a.dest==city.airport.iata);
            else
                return a.origin==d.airport.iata || a.dest == d.airport.iata;
        })  
        .style("stroke", control.options.routeFocusStroke)
        .style("stroke-width", 
            control.options.routeFocusStrokeWidth * ((city && 
                city.airport.iata != d.airport.iata) ? control.options.routeMagnify : 1));
        
        // hide the other routes and cities
        hideRoutes (d);
    }
    
    function cityMouseOver(elem,d) {

        // just consider visible cities
        if (elem.style("display") != 'none') {
            
            var city = clickCity();
            elem.attr("r",control.options.nodeFocusRadius)
                .style("fill",control.options.nodeFocusFill);
            
            cityFilter(city,d);

            // do the barchart by carrier
            var sel = {airport: d.airport.iata };
            if ( city && d.airport.iata != city.airport.iata) 
                sel = {eitherWay: {dest:d.airport.iata, origin:city.airport.iata }};
                
            doDelayBars( createDelayBars(control,"carrier", sel) , sel);
        }
    }

    function clickCity() {
        return control.clicked && control.clicked.type == 'city' ? control.clicked.d : null;
    }
    function showRoutes (city) {
        // show routes featuring this city
        arcPaths.filter( function(a) { 
                return city ? (a.origin == city.airport.iata || a.dest == city.airport.iata) : true;
        })  
        .style("display", 'inline')
        .style("stroke-width", control.options.routeStrokeWidth)
        .style("stroke", control.options.routeStroke)
        
        // show cities there is a route to/from this city
        circle.filter ( function (a) {

            return city ? (a.airport.iata == city.airport.iata || 
                     sumResults (control, 
                          {stop: true, 
                           eitherWay:{ dest: a.airport.iata , origin: city.airport.iata}}).totalFlights > 0) : true;
        })
        .style("display", 'inline');
        
    } 
    function hideRoutes (city) {
        // hide routes not featuring this city
        
        arcPaths.filter( function(a) { 
                return !(a.origin == city.airport.iata || a.dest == city.airport.iata);
        })  
        .style("display", 'none');
        
        // hide cities there is no route to/from this city
        circle.filter ( function (a) {
            return !(city ? (a.airport.iata == city.airport.iata || 
                     sumResults (control, 
                          {stop: true, 
                           eitherWay:{ dest: a.airport.iata , origin: city.airport.iata}}).totalFlights > 0) : true);
        })
        .style("display", 'none');
        
    }
    function hideRoutesNotServed (carrier) {
        // hide routes not featuring this carrier

        arcPaths.filter( function(a) { 
                var result = sumResults (control, {stop : true, carrier:carrier,route:a});
                return (result.totalFlights == 0 );
        })  
        .style("display", 'none');
        
        // hide cities there is no route to/from this city by this carrier and apply currently clicked if any
        circle.filter ( function (a) {
            var sel = {stop : true, carrier:carrier, airport:a.airport.iata};
            var city = clickCity();
            if (city) {
                // never want to hide the selected city
                if (city.airport.iata == a.airport.iata) return false;
                sel.eitherWay = {dest:city.airport.iata,origin:a.airport.iata};
            }
            var result = sumResults (control, sel);
            return (result.totalFlights == 0 );
        })
        .style("display", 'none');
    }  
    function describeRoute(route) {
        var s = describePerformance(sumResults(control, {route:route}));
        s.splice(0,0,route.dest + '-->' + route.origin);
        return s;
    }
    function describeAirport(iata,select) {
        var s= describePerformance(sumResults(control, select));
        s.splice(0,0,iata);
        return s;
    }
    function describePerformance(result) {
        var s = [];
        s.push(result.totalFlights + " flights (" + result.totalDelayed + " delayed > 10 mins)" );
        s.push(Math.round(result.dDelayMins/result.totalDelayed) + " mins late on average");
        s.push(Math.round(result.totalDelayed*100/result.totalFlights) + "%flights over 10 mins late");
        return s;
    }
    
    function getAirportDetails (airport) {
       return airport.name + "(" + airport.iata + "),"+airport.city+","+airport.state;
    }
    
    doDelayBars(createDelayBars(control,"carrier"));
    
    
    function createDelayBars (control,dimension,selection) {
        // filter and calculate data for bars
        var data = [], sel = selection || {};
        // first slice by selection
        var u = sumResults (control,sel);
        // maybe need to filter everything by clicked city
        var city = clickCity();
        var universe = city ? sumResults (control, {airport: city.airport.iata, sliced: u.sliced}) : u;
        // now our universe of data has already been sliced
       
        if (dimension == "carrier"){
            // only one supported for now
            var carriers = control.clean.carriers;
            for (var i =0 ; i < carriers.length ; i++ ) {
                var c=carriers[i];
                var result = sumResults (control, {carrier: c.carrier, sliced: universe.sliced});                
                if (result.totalFlights) {
                    // some data, so add to output
                    data.push ({carrier:c.carrier, result: result});
                }
            }
        }
        
        return data;    
    }
    
    // carrier performance
    function doDelayBars(data,selection) {

        var margin = {top: 14, right: 4, bottom: 10, left: 30},
            barWidth = control.options.barWidth - margin.left - margin.right,
            barHeight = control.options.barHeight- margin.top - margin.bottom;

        var known = control.package.knownAirports, sel = selection || {};
        data.sort(function(a,b) { 
            return +b.result.totalFlights - a.result.totalFlights; 
        }) ;
        // do some heading summary counts
        var s =[],p, totalFlights=0, totalDelayed=0;
        for (var i=0; i < data.length; i++) {
            totalFlights += data[i].result.totalFlights;
            totalDelayed += data[i].result.totalDelayed;
        }
        
        var total = " (" +totalFlights + " flights observed)";
        
        if(sel.airport) { 
            s.push ( getAirportDetails( known[sel.airport] ) + total );
        }
        else if (sel.eitherWay) {
            s.push(sel.eitherWay.origin + "<->" +sel.eitherWay.dest + total);
        }
        else if (sel.route) {
            s.push(sel.route.origin + "->" +sel.route.dest + total);
        }
        else {
            if (clickCity()) 
                s.push ( getAirportDetails( known[clickCity().airport.iata] ) + total );
            else
                s.push("All Airports" + total);
        }
        
        p = totalDelayed/totalFlights;
        s.push(Math.round(p*100) + "%flights over 10 mins late");
        d3.select("#barsummary").html(s.join(":"));

        //-- seems easiest just to delete the old one
        //-- the flights data
        d3.select('#barscrap').remove();
        
        var c = d3.select("#barheading")
          .append("div")
            .attr('id','barscrap')
            .style('width',control.options.barWidth*2 + 'px');
        
        var carrierbar = c.append("svg")
            .attr("width", control.options.barWidth*2)
            .attr("height", control.options.barHeight);
        
        addBars ("total flights",carrierbar,margin.left, "",
                    function(d) {return d.result.totalFlights; } 
                );
        addBars ("%flights > 10 mins late", carrierbar,margin.left + control.options.barWidth, "%",
                    function(d) {return 100*d.result.totalDelayed/d.result.totalFlights; } 
                );
        
        function addBars(title, cb, left,apptxt, f) {

            var g0 = cb.append("g")
              .append("text")
                .attr("class","dataprogress")
                .text(title)
                .attr("transform", "translate(" + left + "," + 12 + ")");
                
                
            var g1 = cb.append("g")
                .attr("transform", "translate(" + left + "," + margin.top + ")");
        
            var x = d3.scale.linear().range([0, barWidth]),
                y = d3.scale.ordinal().rangeRoundBands([0, barHeight], .1);

            x.domain([0, d3.max(data, function(d) { return f(d)  })]);
            y.domain( 
                data.map( function(d) { 
                    return d.carrier; 
                })
            );
    
            var xAxis = d3.svg.axis().scale(x).orient("top").tickSize(-barHeight),
                    yAxis = d3.svg.axis().scale(y).orient("left").tickSize(0);
        
            var barData = g1.selectAll("g.bar")
                .data(data);
            
            var bar = barData.enter().append("g")
                .attr("class", "bar")
                .style("color",control.options.barFill)
                .attr("transform", function(d) { 
                    return "translate(0," + y(d.carrier) + ")";
                });
        
            bar.append("rect")
                .attr("width", function(d) { 
                    return x(f(d)); 
                })
                .attr("height", y.rangeBand())
                .on("mouseover", function(d){
                    
                    d3.select(this)
                        .style("fill",control.options.barFocusFill);
                    barnames.filter ( function (a) { 
                            return a.carrier == d.carrier; } )
                        .attr("class","showname");
                    
                    hideRoutesNotServed (d.carrier);
                  
                })
                .on("mouseout", function(d){
                    d3.select(this)
                        .style("fill",control.options.barFill);
                    d3.selectAll(".showname")
                        .attr("class","hidename");
                    showRoutes (clickCity());
                });
        
            bar.append("text")
                .attr("class", "value")
                .attr("x", function(d) { 
                    return x(f(d)); 
                })
                .attr("y", y.rangeBand() / 2)
                .attr("dx", -3)
                .attr("dy", ".35em")
                .attr("text-anchor", "end")
                .text(function(d) { 
                    return Math.round(f(d))+ apptxt;
                });

            var barnames = bar.append("g")
              .append("text")
                .text(function (d) { 
                    var c = control.clean.knownCarriers[d.carrier];
                    return  c.name + "(" + d.carrier + ")" ;
                })
                .attr("class","hidename")
                .attr("x", function(d) { 
                    return 0; 
                })
                .attr("y", y.rangeBand() / 2);

            g1.append("g")
                .attr("class", "y axis")
                .call(yAxis);
            
            return g1;
        }

    }      
}
function sumResults (control,selection) {
    
    // this is a general purpose summer
    var journeys = control.clean.journeys, 
        known = control.package.knownAirports,
        routes= control.clean.routes,
        carrier = control.clean.carriers,
        knownCarriers = control.clean.knownCarriers,
        knownRoutes = control.clean.knownRoutes;
    
    var slices = [];        
    
    // maybe we can slice it
    if (selection.sliced) {
        slices.push(selection.sliced);
    }
    else if (selection.airport) {
        if (selection.airport)slices.push (known[selection.airport].slice);
    }
    else if (selection.destAirport || selection.originAirport) {
        if (selection.destAirport)slices.push (known[selection.destAirport].slice);
        if (selection.originAirport)slices.push (known[selection.originAirport].slice);
    }
    else if (selection.route) {
        var r = knownRoutes[routeKey(selection.route)];
        // its possible that we have a 'one way route'
        if (r)slices.push(r.slice);
    }
    else if (selection.eitherWay) {
        var r = knownRoutes[routeKey(selection.eitherWay)];
        if (r)slices.push(r.slice);
        
        var r = knownRoutes[routeKey( {dest:selection.eitherWay.origin,origin:selection.eitherWay.dest})];
        if (r)slices.push(r.slice);

    }
    else if (selection.carriers) {
        slices.push(knownCarriers[selection.carrier].slice);
    }
    else {
        slices.push(journeys);
    }
    
    var result = {totalFlights: 0, totalDelayed:0, aDelayMins:0, dDelayMins:0 , sliced:[]};
    
    for ( var k=0; k < slices.length ; k++) {
        for ( var i = 0; i < slices[k].length  && (!selection.stop || result.totalFlights == 0 ); i++) {
            var journey = slices[k][i];
            //this flight is from the requested carrier
            if (!selection.carrier || (journey.carrier.carrier == selection.carrier)) {
                // and the route matches
                if (!selection.route || (journey.route.dest == selection.route.dest && journey.route.origin == selection.route.origin) ) {
                    // good on airport
                    if (!selection.destAirport || journey.route.dest == selection.destAirport ) {
                        if (!selection.originAirport || journey.route.origin == selection.originAirport) {
                            if (!selection.airport || journey.route.origin == selection.airport || journey.route.dest == selection.airport) {
                                if (!selection.eitherWay || 
                                    ((journey.route.origin == selection.eitherWay.origin && journey.route.dest == selection.eitherWay.dest) || 
                                    (journey.route.dest == selection.eitherWay.origin && journey.route.origin == selection.eitherWay.dest))) {
                                // we qualify
                                    result.totalFlights += +journey.flight.totalFlights;
                                    result.sliced.push(journey);
                                    if (journey.late) {
                                        result.totalDelayed += +journey.late.totalDelayed;
                                        result.aDelayMins += +journey.late.aDelayMins;
                                        result.dDelayMins += +journey.late.dDelayMins;
                                        
                                    }
                                }
                            }                 
                        }
                    }
                }
            }
            
        }
    }
    return result;
}
function consolidateData(control) {
    // create a consolidated data sets
    control.package.knownAirports = makeKnownAirports(control);
    control.clean = {};
    control.clean.routes= makeGoodRoutes(control);
    control.clean.airports= makeRelevantAirports (control);
    makeGoodCarriers(control);
    control.clean.journeys = makeGoodJourneys(control);
    addCarrierCodes (control);
    makeSlices(control);
    
    return control;
}

function findCarrier (flight,carriers) {
    // find the carrier mentioned in the known carriers
    var tc = flight.carrier.hasOwnProperty("carrier") ? flight.carrier.carrier : flight.carrier;
    for (var i =0; i < carriers.length;i++) {
        if (carriers[i].carrier == tc ) {
            return carriers[i];
        }
    }
    return null;
}
// --normalize data
function makeGoodCarriers (control) {
// make list of known carriers - later we'll get this from some kind of database
    var carriers = [], flights = control.package.carriers,
        knownRoutes=control.clean.knownRoutes,
        knownCarriers = {};
        
    for ( var i = 0; i < flights.length ; i++ ) {
        var flight = flights[i];
        if (knownRoutes[routeKey(flight)]) {
            if (!knownCarriers[flight.carrier]) {
                var carrier = {carrier:flight.carrier};
                carriers.push(carrier);
                knownCarriers[carrier.carrier] = carrier;
            }
        }
    }
    control.clean.carriers = carriers;
    control.clean.knownCarriers = knownCarriers;
    return carriers;
}
function findJourney (flight,journeys) {
    for (var i=0; i< journeys.length ; i++) {
        if (journeys[i].carrier.carrier == flight.carrier && 
            journeys[i].route.dest == flight.dest && 
            journeys[i].route.origin == flight.origin) return journeys[i];
    }
    return null;
}
function makeGoodJourneys(control) {
    // merge the lates and the flights
    var journeys = [], lates = control.package.lates, 
        flights = control.package.carriers, 
        knownRoutes = control.clean.knownRoutes,
        carriers = control.clean.carriers,
        knownCarriers = control.clean.knownCarriers;
        
    for (var i=0;i<flights.length;i++) {
        var flight = flights[i];
        var route = knownRoutes[routeKey(flight)];
        if (!route) 
            console.log ("route missing for flight " + JSON.stringify(flight)); 
        else {
            var carrier = knownCarriers[flight.carrier];
            if (!carrier) 
                console.log ("woah - carrier missing for flight " + JSON.stringify(flight)); 
            else {
                journeys.push({ carrier: carrier, route: route, flight:flight,late:null });
            }
        }
    }
   
    // now add the lates
    for (var i=0;i<lates.length;i++) {
        var late = lates[i];
        var journey = findJourney(late,journeys);
        if (!journey) {
            console.log ("journey missing for late " + JSON.stringify(late)); 
        }
        else {       
            if(journey.late) 
                console.log ("woah - duplicate late " + JSON.stringify(late));
            else
                journey.late = late;
        }
    }
    return journeys;
    
}

function makeSlices (control) {
    // for optimizing a bit
    var journeys = control.clean.journeys, 
        known = control.package.knownAirports, w = ['dest','origin'],
        knownRoutes = control.clean.knownRoutes,
        knownCarriers = control.clean.knownCarriers, 
        carriers = control.clean.carriers;
    
    // airports
    for ( var j=0; j < w.length ; j++) {
        for ( var i=0; i < journeys.length ; i++) {
            var journey = journeys[i];
            var slice = known[journey.route[w[j]]].slice;
            if(!slice) {
                var slice = [];
                known[journey.route[w[j]]].slice = slice;
            }
            slice.push(journey);
        }
    }
    // routes
    for ( var i=0; i < journeys.length ; i++) {
        var journey = journeys[i];
        var route = knownRoutes[routeKey(journey.route)];
        var slice = route.slice;
        if (!slice) {
            slice = [];
            route.slice = slice;
        }
        slice.push(journey);
    }
    // carriers
    for ( var i=0; i < journeys.length ; i++) {
        var journey = journeys[i];
        var carrier = knownCarriers[journey.carrier.carrier];
        var slice = carrier.slice;
        if (!slice) {
            slice = [];
            carrier.slice = [];
        }
        slice.push(journey);
    }

}
function routeKey (route) {
    return route.origin + "_" + route.dest;
}
function makeGoodRoutes (control) {
    // drop routes that we dont know where the airport is
    var flights = control.package.carriers;
    var known = control.package.knownAirports;
    var knownRoutes = {};
    var good = [];
    
    for (var i=0; i < flights.length; i++) {
        var flight = flights[i];
        if (known[flight.dest] && known[flight.origin]) {
            var route = knownRoutes[routeKey(flight)];   
            if (!route) {
                route = { dest: flight.dest, origin: flight.origin };
                good.push (route);
                knownRoutes[routeKey(flight)] = route;
            }
        }
        else
            console.log ("unknown airport:dropping route " + JSON.stringify(flight));
    }
    control.clean.knownRoutes = knownRoutes;
    return good;
    
}
 
function initialize () {
   
    var initPromise = $.Deferred();

    getTheData().done( function (package) {  
        var control = {};
        control.package = package;
        control.divName = "#chart";
        control.barName = "#barheading"
    // set some default options
        control.options = $.extend({
            radius : .006,
            fontSize : 14,
            labelFontSize : 8,
            nodeLabel : null,
            width : $(control.divName).width(),
            nodeResize : "",
            styleColumn : null,
            styles : null,
            linkName : null,
            nodeFocus: true,
            barFill: "SteelBlue",
            barFocusFill: "White",
            nodeFocusRadius: 25,
            nodeFocusFill: "FireBrick",
            routeFocusStroke: "FireBrick",
            routeFocusStrokeWidth: "3",
            circleFill: "DimGray",
            routeStroke: "SteelBlue",
            routeStrokeWidth: "1",
            labelOffset: "5",
            clickHack : 200,
            height : 600,
            barWidth:$(control.barName).width()/2,
            barHeight:300,
            mapScale:900,
            routeMagnify: 4,
            nodeClickFill: 'Sienna'
        }, package.options);
   
        control.color = d3.scale.category20();
        
        // scale map from normal size
        var p = control.options.mapScale/1200;
        control.options.radius *= p;
        control.options.routeStrokeWidth *= p;
        control.options.routeFocusStrokeWidth *= p;
        control.options.nodeFocusRadius *= p;
        
        control.svg = d3.select(control.divName)
            .append("svg:svg")
            .attr("width", control.options.width)
            .attr("height", control.options.height);
            
        control.projection = d3.geo.azimuthal()
            .mode("equidistant")
            .origin([-98, 38])
            .scale(control.options.mapScale)
            .translate([control.options.width/2, control.options.height/2]);
        
       control = consolidateData(control);
       initPromise.resolve(control);
    })
    .fail( function (error) {
       initPromise.reject(error); 
    });
    
    return initPromise.promise();
}

function getTheData() {
    var massage = $.Deferred();
    var proxy = "proxyphp.php";
    var proxy = "http://xliberation.com/cdn/php/proxyphp.php";
    makeLight("fusion","busy");

    var mapsPromise = getPromiseData("http://xliberation.com/cdn/data/us-states.json",proxy,"maps"),
        latesPromise = getTheLateData("delays"),
        codesPromise = getTheCarrierCodes("codes"),
        carriersPromise  = getTheCarrierData("flights"),
        airportsPromise = getTheAirportData("airports");
    
    var package  = {};
    // override any default options
    package.options = {};

    carriersPromise.done( function (carriers) {
        package.carriers = makeBetterData(carriers); 
    });

    codesPromise.done( function (codes) {
        package.codes = makeBetterData(codes);
    });
    
    airportsPromise.done( function (airports) {
        package.airports = makeBetterData(airports);
    });
    
    latesPromise.done( function (lates) {
        package.lates = makeBetterData(lates);
    });

    mapsPromise.done( function (states) {
        package.states = states;
    });
    
    $.when(mapsPromise,
            latesPromise,
            carriersPromise,
            airportsPromise,
            codesPromise)
        .done(function(states,lates,routes,carriers,airports,codes) {
            makeLight("fusion","idle");
            massage.resolve(package);
        })
        .fail ( function(error) {
            $('#status').html(error);
            massage.reject(error);
            makeLight("fusion","failed");
        });

    return massage.promise();
}
function makeBetterData(fusionData) {
    var newObs = [];

    for ( var i= 0; i < fusionData.rows.length;i++) {
        var newOb = {}; 
        for ( var j = 0; j < fusionData.columns.length ; j++) {
            newOb[fusionData.columns[j]] = fusionData.rows[i][j];
        }
        newObs.push(newOb);
    }
    return newObs;
}


// take the raw data and prepare it for d3

function getTheLateData() {
    var s = "SELECT Carrier AS carrier,Dest AS dest,Origin AS origin,";
    s +="SUM(ArrDelayMinutes) AS aDelayMins,SUM(DepDelayMinutes) AS dDelayMins,COUNT(RowId) AS totalDelayed";
    var sql = [s];
    sql.push("FROM " + "1aoTFJygMLOD0r-QsaKG5-4375VwQpwuGb18dMGw");
    sql.push("WHERE ArrDelayMinutes > '10' AND Year = '2012'");
    sql.push("GROUP by Dest,Origin,Carrier");
    //sql.push("LIMIT 200");
    return getFusionData(sql,"delays");
}

function getTheCarrierData() {
    var s = "SELECT Carrier AS carrier,Dest AS dest,Origin AS origin,";
    s +="COUNT(RowId) AS totalFlights";
    var sql = [s];
    sql.push("FROM " + "1aoTFJygMLOD0r-QsaKG5-4375VwQpwuGb18dMGw");
    sql.push("WHERE Year = '2012'");
    sql.push("GROUP by Dest,Origin,Carrier");
    //sql.push("LIMIT 100");
    return getFusionData(sql,"flights");
}

function getTheAirportData() {
    var s = "SELECT *";
    var sql = [s];
    sql.push("FROM " + "1Ug6IA-L5NKq79I0ioilPXlojEklytFMMtKDNzvA ORDER BY iata");
    //sql.push("LIMIT 1000");
    return getFusionData(sql,"airports");
}
function getTheCarrierCodes() {
    var s = "SELECT *";
    var sql = [s];
    sql.push("FROM " + "1pvt-tlc5z6Lek8K7vAIpXNUsOjX3qTbIsdXx9Fo ORDER BY carrier");
    //sql.push("LIMIT 1000");
    return getFusionData(sql,"codes");
}

function getFusionData (sql,light) {
    var key='AIzaSyB4smrtU7ZyaYl0s7SBuO9I7Iv4NzFezvQ';
    var url = ['https://www.googleapis.com/fusiontables/v1/query?'];
    url.push('key=' + key);
    url.push('&sql=' + encodeURIComponent(sql.join(" ")));
    // promise will be resolved when done
    return getPromiseData(url.join(""),undefined,light); 
}

// general deferred handler for getting json data and creating promise
function getPromiseData(url,proxyUrl,light){
    
    var deferred = $.Deferred();
    var u = url;
   
    if (proxyUrl ) {
            u = proxyUrl+"?url="+encodeURIComponent(url);
    }
    else {
            // if not proxied, needs to be jSONp
            u = u + "&callback=?"     
    }
    
    backOffJson (deferred,u,light);
    return deferred.promise();

}
function backOffJson (defer,u, light,tries) {
    // uses exponential back off
    tries = tries || 0;
    if(light)makeLight(light,"busy");
    $.getJSON(u, null, 
        function (data) {
            if (data.error) {
                if (data.error.code == 503) {
                    // a 503 error - so i'll back off
                    tries ++;
                    if (tries <= 5) {
                        // try again
                        var hangAroundFor = (Math.pow(2,tries)*1000) + (Math.round(Math.random() * 1000));
                        if (light)makeLight(light,"lemon");
                        setTimeout(function() {
                                        backOffJson (defer,u,light,tries);
                                    },hangAroundFor);
                    }
                    else { 
                        defer.reject(data); 
                    }
                }
                else {
                  defer.reject(data);
                }
            }
            else {
                defer.resolve(data);
            }
    })
    .error(function(res, status, err) {
        defer.reject("error " + err + " for " + u);
    });
    
    defer.promise()
        .done (function () {
            if (light) makeLight(light,"idle");
        })
        .fail( function () {
            if(light)makeLight(light,"failed");
        });
        
    return defer;
}

function makeLight (what,img) {
     d3.select('#'+what+'busy')
        .attr('src', "//xliberation.com/cdn/img/" +img + ".gif"); 
}

function addCarrierCodes (control) {
    var codes = control.package.codes;
    var carriers = control.clean.carriers;
    for (var i =0; i < codes.length ; i++) {
       for (var j=0; j < carriers.length ; j++) {
           if (carriers[j].carrier == codes[i].carrier) carriers[j].name = codes[i].name;
       } 
    }
}
function makeRelevantAirports (control) {
    // this will make an array of relevant airports filtered from knownAirports
    var relevant = [] ;
    var package = control.package;
    var known = package.knownAirports;
    var routes = control.clean.routes;
    for (var i=0; i < routes.length ; i++) {
        addRelevant (routes[i],"origin",known, relevant);
        addRelevant (routes[i],"dest",known, relevant);
    }
    return relevant;
}
function addRelevant (route,key,known,relevant) {
    var a= route[key],ap=null;
    if (known[a]) {
        for (var i=0;i<relevant.length && !ap ;i++) {
            if (relevant[i].airport.iata == a) ap = relevant[i];
        } 
        if (!ap) {
            ap = { airport : known[a] }; 
            relevant.push (ap);
        }
    }
    else console.log ('missing airport' + a);   
    return ap;
}

function makeKnownAirports(control) {
        var knownAirports = {}, package = control.package;
        // make an airport object indexable by iata code for each airport
        for ( var i=0;i<package.airports.length;i++) {
            var iata = package.airports[i].iata;
            var airport = knownAirports[iata] = package.airports[i];
            airport.location = [airport.longitude, airport.latitude];
            airport.position = control.projection(airport.location);
        }
        return knownAirports;  
}

</script></head>
<body>
    <div  style= "float:right;width:65%;">
        <div id="chart" class="chart" style="width:100%;">
        </div>

     </div>
     <div style="float:left;width:30%;">
     
     <div class="infobar" style="width:100%;">
            <img class="creditimage" 
             src="http://lh3.googleusercontent.com/-MjqAZOIzLvk/AAAAAAAAAAI/AAAAAAAAAAA/HceWILx6kw4/s49-c/photo.jpg"></img>
            <div class="creditintro">
                <strong>Bruce's comments</strong>
                <br>This pulls Google Fusion data into a d3.js interactive chart. Source data is 2012 all US flight data.
                Hover over cities, routes or carrier bars. Click on city to freeze/unfreeze available routes.
            </div>
            <div class="creditbody">Ackowledgements<br>
                <a href='http://bost.ocks.org/mike/'>Mike Bostok for d3.js</a>
                <a href='http://mbostock.github.io/d3/talk/20111116/airports.html'>and the state paths</a><br>
                <a href='https://datamarket.azure.com/dataset/oakleaf/us_air_carrier_flight_delays_incr'>Oakleaf Systems flight data on Azure</a><br>
                <a href='http://ramblings.mcpher.com'><strong>For more stuff like this visit Excel Liberation</strong></a><br>
            </div>
            <div class="dataprogress">
                A lemon below means Fusion is busy - it will try again shortly<br>
                <strong>getting Google Fusion data</strong>..(Patience..1 million rows) <img id="fusionbusy" class="busy" src="//xliberation.com/cdn/img/idle.gif"></img><br>
                maps<img id="mapsbusy" class="busy" src="//xliberation.com/cdn/img/lemon.gif"></img>
                flights<img id="flightsbusy" class="busy" src="//xliberation.com/cdn/img/lemon.gif"></img>
                airports<img id="airportsbusy" class="busy" src="//xliberation.com/cdn/img/lemon.gif"></img>
                delays<img id="delaysbusy" class="busy" src="//xliberation.com/cdn/img/lemon.gif"></img>
                codes<img id="codesbusy" class="busy" src="//xliberation.com/cdn/img/lemon.gif"></img>
                d3<img id="d3busy" class="busy" src="//xliberation.com/cdn/img/lemon.gif"></img>
            </div>
     
            <div id = "status" class= "status">
            </div>
            <div class="social">
                <a href="https://twitter.com/share" class="twitter-share-button" data-text="d3.js and Google Fusion Table Integration" data-via="brucemcpherson" data-count="none" data-hashtags="d3js">Tweet</a>
                <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>
                <a href="https://twitter.com/brucemcpherson" class="twitter-follow-button" data-show-count="false" data-show-screen-name="false">Follow @brucemcpherson</a>
                <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>
             
                <!-- Place this tag where you want the +1 button to render. -->
                <div class="g-plusone" data-size="small" data-annotation="inline" data-width="200"></div>
            
                <!-- Place this tag after the last +1 button tag. -->
                <script type="text/javascript">
                  (function() {
                    var po = document.createElement('script'); po.type = 'text/javascript'; po.async = true;
                    po.src = 'https://apis.google.com/js/plusone.js';
                    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(po, s);
                  })();
                </script>      
            </div>
        </div>
     
     
     
     
        <div id = "barheading" class="barheading">
            <div id="barsummary" class="bartitles"></div>
            </div>
        </div>
    </div>
 </body>
</html>
