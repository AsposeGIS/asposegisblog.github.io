---
layout: post
title: Geometry analysis features in Aspose.GIS
---

<a href="https://products.aspose.com/gis">Aspose.GIS</a> doesn't only convert geospatial data between file formats and allows to create and read the data, it also allows some analysis of the data. Here I'll describe gow Aspose.GIS can be used for geospatial data analysis in more detail. 

Geospatial features are represented as subclasses of <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry">Geometry</a> class - <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/point">Point</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/surface">Surface</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/linestring">LineString</a> (polyline), <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/linearring">LinearRing</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/polygon">Polygon</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/multipoint">MultiPoint</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/multilinestring">MultiLineString</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/multipolygon">MultiPolygon</a>, and <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometrycollection">GeometryCollection</a>. The methods used for analysis are provided by Geometry class and thus are availible for any geospatial feature. In this article I'll describe what methods are provided by Geometry class for analysis.

## Properties

For the beginning, let'w review what properties the Geometry class provides:

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/coordinatedimension">CoordinateDimension</a>
Gets number of coordinate dimensions of this geometry. I.e. 2 if there are only X and Y coordinates, 3 if there are X, Y and Z or M coordinate, etc.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/dimension">Dimension</a>
Gets topological dimension of the geometry - i.e. if it is a point, line or surface.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/geometrytype">GeometryType</a>
Gets type of geometry - essentially name of the class of the instance - if it is a Polygon, GeometryType will return <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometrytype">GeometryType</a>.Polygon.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/hasm">HasM</a>
Gets wether this geometry has M coordinate. 
An M coordinate (measure) is a value that conveys information about a geographic feature and that is stored together with the coordinates that define the feature's location.
For example, suppose that you are representing highways in your application. If you want your application to process values that denote linear distances or mileposts, you can store these values along with the coordinates that define locations along the highway.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/hasz">HasZ</a>
Gets wether this geometry has Z coordinate (elevation).

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/isempty">IsEmpty</a>
Gets wether this geometry is empty (i.e. contains no geospatial data).

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/issimple">IsSimple</a>
Gets wether this geometry is simple in <a href="http://www.opengeospatial.org/standards/sfa">SFA</a> terms.
Here is an example:
```csharp
LineString lineString = new LineString();
bool simple = lineString.IsSimple; // simple == true
lineString.AddPoint(0, 0);
lineString.AddPoint(1, 0);
simple = lineString.IsSimple; // simple == true
lineString.AddPoint(0.5, 0);
simple = lineString.IsSimple; // simple == false (line string crosses itself)
```

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/isvalid">IsValid</a>
Gets wether this geometry is valid.
Here is an example:
```csharp
LinearRing linearRing = new LinearRing();
linearRing.AddPoint(0, 0);
linearRing.AddPoint(0, 1);
linearRing.AddPoint(1, 0);
bool valid = linearRing.IsValid; // valid == false
linearRing.AddPoint(0, 0);
valid = linearRing.IsValid; // valid == true
```

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/null">Null</a>
Static method that returns null geometry.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/spatialreferencesystem">SpatialReferenceSystem</a>
Gets <a href="https://apireference.aspose.com/net/gis/aspose.gis.spatialreferencing/spatialreferencesystem">SpatialReferenceSystem</a> of this Geometry. It can be null, if SRS is not known. Assigning new SRS won't perform any transformation, only reference will be changed.

## Methods for basic property analysis

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/getarea">GetArea</a>
Gets area of the geometry, if it is a collection, then sum of all areas.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/getdistanceto">GetDistanceTo</a>
Gets closest distance from this geometry to specified other geometry. If at least one of geometries is empty, returns -1.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/getlength">GetLength</a>
Gets length of the geometry. If it is a Polygon, then returns perimeter. If it is a collection, then returns sum of all lengths.

## Methods for boolean relationships

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/coveredby">CoveredBy</a>
Returns true if this geometry is spatially covered by another geometry, otherwise false.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/covers">Covers</a>
Returns true is this geometry spatially covers the other geometry. Essentially same as CoveredBy with this and other geometries swapped.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/crosses">Crosses</a>
Tests wether two geometries cross, i.e. they have some but not all interior points in common, and dimension of the intersection is smaller than dimension of at least one of geometries, i.e. for example intersection is a line while one of geometries is a surface. 
Here is an example:
```csharp
var geometry1 = new LineString();
geometry1.AddPoint(0, 0);
geometry1.AddPoint(2, 2);

var geometry2 = new LineString();
geometry2.AddPoint(1, 1);
geometry2.AddPoint(3, 3);

Console.WriteLine(geometry1.Crosses(geometry2)); // False

var geometry3 = new LineString();
geometry3.AddPoint(0, 2);
geometry3.AddPoint(2, 0);

Console.WriteLine(geometry1.Crosses(geometry3)); // True
```

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/disjoint">Disjoint</a>
Returns true if two geometries have no common points.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/intersects">Intersects</a>
The reverse of Disjoint.
Here is an example:
```csharp
var geometry1 = new Polygon(new LinearRing(new[]
{
    new Point(0, 0),
    new Point(0, 3),
    new Point(3, 3),
    new Point(3, 0),
    new Point(0, 0),
}));

var geometry2 = new Polygon(new LinearRing(new[]
{
    new Point(1, 1),
    new Point(1, 4),
    new Point(4, 4),
    new Point(4, 1),
    new Point(1, 1),
}));

Console.WriteLine(geometry1.Intersects(geometry2)); // True
Console.WriteLine(geometry2.Intersects(geometry1)); // True

// 'Disjoint' is opposite to 'Intersects'
Console.WriteLine(geometry1.Disjoint(geometry2)); // False
```

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/overlaps">Overlaps</a>
Tests wether geometries overlap, as in they have some interior points in common and intersection has same dimension as geometries, i.e. for example geometries are surfaces and intersection is a surface too. Always false if geometries are of different dimensions.
Here is an example:
```csharp
var geometry1 = new LineString();
geometry1.AddPoint(0, 0);
geometry1.AddPoint(0, 2);

var geometry2 = new LineString();
geometry2.AddPoint(0, 2);
geometry2.AddPoint(0, 3);

Console.WriteLine(geometry1.Overlaps(geometry2)); // False

var geometry3 = new LineString();
geometry3.AddPoint(0, 1);
geometry3.AddPoint(0, 3);

Console.WriteLine(geometry1.Overlaps(geometry3)); // True
```

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/spatiallycontains">SpatiallyContains</a>
Tests if this geometry spatially contains another.
Here is an example:
```csharp
var geometry1 = new Polygon();
geometry1.ExteriorRing = new LinearRing(new[]
{
    new Point(0, 0),
    new Point(0, 4),
    new Point(4, 4),
    new Point(4, 0),
    new Point(0, 0),
});
geometry1.AddInteriorRing(new LinearRing(new[]
{
    new Point(1, 1),
    new Point(1, 3),
    new Point(3, 3),
    new Point(3, 1),
    new Point(1, 1),
}));

var geometry2 = new Point(2, 2);

Console.WriteLine(geometry1.SpatiallyContains(geometry2)); // False

var geometry3 = new Point(0.5, 0.5);
Console.WriteLine(geometry1.SpatiallyContains(geometry3)); // True

// 'a.SpatiallyContains(b)' equals to 'b.Within(a)'
Console.WriteLine(geometry3.Within(geometry1)); // True
```

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/spatiallyequals">SpatiallyEquals</a>
Tests if two geometries are spatially equal. Essentially, tests if two geometries occupy same space when projected onto two-dimensional space. Here is an example:
```csharp
var geometry1 = new MultiLineString
{
    new LineString(new [] { new Point(0, 0), new Point(1, 1) }),
    new LineString(new [] { new Point(1, 1), new Point(2, 2) }),
};

var geometry2 = new LineString(new[]
{
    new Point(0, 0), new Point(2, 2),
});

Console.WriteLine(geometry1.SpatiallyEquals(geometry2)); // True

geometry2.AddPoint(3, 3);
Console.WriteLine(geometry1.SpatiallyEquals(geometry2)); // False
```

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/touches">Touches</a>
Returns true if two geometries have at lease one common boundary points, but no interior points.
Here is an example:
```csharp
var geometry1 = new LineString();
geometry1.AddPoint(0, 0);
geometry1.AddPoint(2, 2);

var geometry2 = new LineString();
geometry2.AddPoint(2, 2);
geometry2.AddPoint(3, 3);

Console.WriteLine(geometry1.Touches(geometry2)); // True
Console.WriteLine(geometry2.Touches(geometry1)); // True

var geometry3 = new Point(2, 2);
Console.WriteLine(geometry1.Touches(geometry3)); // True

var geometry4 = new LineString();
geometry4.AddPoint(1, 1);
geometry4.AddPoint(4, 4);

Console.WriteLine(geometry1.Touches(geometry4)); // False
```

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/within">Within</a>
Tests if geometry is contained by another. Equal to SpatiallyContains with this and other geometries swapped.

## Methods for generation of derivative geometry

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/difference">Difference</a>
Returns geometry that is a difference of this geometry and the argument geometry, containing points from this geometry, that are not contained in argument geometry. 

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/getbuffer">GetBuffer</a>
Returns geometry that contains all points within specified distance from this geometry. Useful when you need a 'border area' around geometry.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/getcentroid">GetCentroid</a>
Returns geometric center of geometry, in layman terms - point at which cutout of the shape can be perfectly balanced on a pin.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/getconvexhull">GetConvexHull</a>
Returns convex hull of the geometry. Null if geometry is empty, point if geometry is a point, is it's two points, then line between points, otherwise ILinearRing that is a convex hull around all points.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/intersection">Intersection</a>
Returns geometry that contains points that are contained in both this geometry and argument geometry.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/symdifference">SymDifference</a>
Returns geometry that contains points that are contained in either this or argument geometry, but not both.

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/union">Union</a>
Returns geometry that is a sum of this and argument geometries, containing points from both geometries.

## General case relation, DE-9IM model.
The <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/relate">Relate</a> method allows one to specify any desired pattern for  <a href="https://en.wikipedia.org/wiki/DE-9IM">DE-9IM</a> intersection matrix. In fact, most of the above relation-deriving methods are wrappers for this method. Method builds DE-9IM matrix for the two geometries and tests if it fits the provided pattern.
Pattern is a string of 9 characters.
Each position represents specific spatial relation:
0 - between interiors of geometries.
1 - between interior of this geometry and boundary of the other.
2 - between interior of this geometry and exterior of the other.
3 - between boundary of this geometry and interor of the other.
4 - between boundaries of the geometries.
5 - between boundary of this geometry and exterior of the other.
6 - between exterior of this geometry and interior of another geometry.
7 - between exterior of this geometry and boundary of another geometry.
8 - between exteriors of the geometries.
Each character can have the following values:
* * - any value
* F - no intersection
* T - any intersection
* 0 - point intersection (shared point)
* 1 - line intersection (shared line segment)
* 2 - surface intersection (shared part of polygon)

For example, an intersection pattern "F0*******" means, that there should not be intersection between geometries interiors and intersection between geometries boundaries must be a point.
Also you can see <a href="http://www.opengeospatial.org/standards/sfa">OpenGIS Simple Features Specification</a> for more details.
Here is an example on how to use it:
```csharp
var geometry1 = new LineString();
geometry1.AddPoint(0, 0);
geometry1.AddPoint(0, 2);

var geometry2 = new LineString();
geometry2.AddPoint(0, 1);
geometry2.AddPoint(0, 3);

// Relate method takes a string representation of DE-9IM matrix
// (Dimensionally Extended Nine-Intersection Model matrix).
// see Simple Feature Access specification for more details on DE-9IM.

// this is the equivalent of 'geometry1.SpatiallyEquals(geometry2)'
Console.WriteLine(geometry1.Relate(geometry2, "T*F**FFF*")); // False

// this is the equivalent of 'geometry1.Disjoint(geometry2)'
Console.WriteLine(geometry1.Relate(geometry2, "FF*FF****")); // False

// this is the equivalent of 'geometry1.Overlaps(geometry2)'
Console.WriteLine(geometry1.Relate(geometry2, "1*T***T**")); // True
```

## Miscellaneous methods

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/roundm">RoundM</a>
Rounds M coordinate to a specified number of fractional digits. 

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/roundxy">RoundXY</a>
Rounds X and Y coordinates to a specified number of fractional digits. 

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/roundz">RoundZ</a>
Rounds Z coordinate to a specified number of fractional digits. 

#### <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/setempty">SetEmpty</a>
Empties the geometry.

Together, these methods allow you to analyse topological relation of geometries, do 'cookie-cutting', compare their areas and a lot more.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-gis">Aspose.GIS GitHub</a> page. There's also <a href="https://twitter.com/AsposeGis">Twitter</a> and <a href="https://www.facebook.com/AsposeGis/">Facebook</a> pages for news on Aspose.GIS.
