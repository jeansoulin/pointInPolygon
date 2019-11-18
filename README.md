# pointInPolygon
comparing 3 javascript solutions
see: https://stackoverflow.com/questions/22521982/check-if-point-is-inside-a-polygon/58784254#58784254

Problem:
"user:3378649" wants to check if an array of point lies within a specific polygon.

    const polygon=   [
        [-73.89632720118, 40.8515320489962],
        [-73.8964878416508, 40.8512476593594],
        [-73.8968799791431, 40.851375925454],
        [-73.8967188588015, 40.851660158514],
        [-73.89632720118, 40.8515320489962]
    ];
    const pointstocheck = [
        [-73.89632720118, 40.8515320489962], [-73.8964878416508, 40.8512476593594], [-73.8968799791431, 40.851375925454],
        [-73.8967188588015, 40.851660158514], [-73.89632720118, 40.8515320489962,]
    ];

Solution 1:

    function isPointInPoly(py, poly){
        for(var c = false, i = -1, l = poly.length, j = l - 1; ++i < l; j = i)
            ((poly[i][1] <= pt[1] && pt[1] < poly[j][1]) || (poly[j][1] <= pt[1] && pt[1] < poly[i].y))
            && (pt[0] < (poly[j][0] - poly[i][0]) * (pt[1] - poly[i][1]) / (poly[j][1] - poly[i][1]) + poly[i][0])
            && (c = !c);
        return c;
    }
    const res1 = pointstocheck.map(p => isPointInPoly(p, polygone);

Solution 2: ray-casting algorithm based on
      http://www.ecse.rpi.edu/Homepages/wrf/Research/Short_Notes/pnpoly.html

    function inside(point, vs) {
      var x = point[0], y = point[1];
      var inside = false;
      for (var i = 0, j = vs.length - 1; i < vs.length; j = i++) {
          var xi = vs[i][0], yi = vs[i][1];
          var xj = vs[j][0], yj = vs[j][1];
          var intersect = ((yi > y) != (yj > y))
            && (x < (xj - xi) * (y - yi) / (yj - yi) + xi);
          if (intersect) inside = !inside;
      }
      return inside;
    };
    const res2 = pointstocheck.map(p => inside(p, polygone);

Solution 3: a sinle-instruction version of 2, using de-structuring of point into [x,y]

    const inside3 = ([x,y], vs) =>
      vs.some(([xi,yi], i, v) =>
        i++ < (v.length-1) && (yi > y) !== (v[i][1] > y) && x < (v[i][0] - xi) * (y - yi) / (v[i][1] - yi) + xi);
    const res3 = pointstocheck.map(p => inside3(p, polygone);
