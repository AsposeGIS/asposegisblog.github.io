<a href="https://products.aspose.com/gis/">Aspose.GIS</a> handles geospatial data in a common way regardless of storage format, as all the GIS data formats store, in principle, the same kind of data. As such, methods to create and check geometric data are not coupled to format and can be used in any case. In this article I will overview how to create geometric data in Aspose.GIS and check its validity.

## Creation of Point, LineString and Polygon
<a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/point">Point</a> is just a point, <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/linestring">LineString</a> is a polyline - a series of connected points, ang <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/polygon">Polygon</a> is basically an area enclosed by closed polyline. As such, Point can be <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/point/constructors/main">instantiated</a> with default values, from another point, or by passing X, Y and optional Z and M coordinates. LineString can be created by instantiating it and then adding points to it - either passing a Point to <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/linestring/methods/addpoint">AddPoint</a>, or just passing coordinates.

Here is an example of creation of Point and LineString:
```csharp
Point point = new Point(40.7128, -74.006);

LineString line = new LineString();

line.AddPoint(78.65, -32.65);

line.AddPoint(-98.65, 12.65);

line.AddPoint(point);
```

Polygon is a bit more complex. It has outer boundary, a closed polyline represented as <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/linearring">LinearRing</a> instance - this class is a LineString that ensures it's closed and doesn't self-intersect -, and it can have multiple inner boundaries, essentially "holes" in its geometry. These are also represented by LinearRing instances. External boundary is placed at <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/polygon/properties/exteriorring">ExteriorRing</a> property, while interior rings are accessed by <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/polygon/methods/addinteriorring">AddInteriorRing</a> and <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/polygon/methods/getinteriorring">GetInteriorRing</a> methods, with only total count represented as <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/polygon/properties/interiorringscount">InteriorRingsCount</a> property. Look at the example on creating a polygon with a hole:
```csharp
Polygon polygon = new Polygon();

LinearRing ring = new LinearRing();
ring.AddPoint(50.02, 36.22);
ring.AddPoint(49.99, 36.26);
ring.AddPoint(49.97, 36.23);
ring.AddPoint(49.98, 36.17);
ring.AddPoint(50.02, 36.22);

LinearRing hole = new LinearRing();
hole.AddPoint(50.00, 36.22);
hole.AddPoint(49.99, 36.20);
hole.AddPoint(49.98, 36.23);
hole.AddPoint(50.00, 36.24);
hole.AddPoint(50.00, 36.22);

polygon.ExteriorRing = ring;
polygon.AddInteriorRing(hole);
```

## Creation of geometry collection
Geometry collection is created in obvious way, create a collection and add elements to it:

```csharp
Point point = new Point(40.7128, -74.006);

LineString line = new LineString();
line.AddPoint(78.65, -32.65);
line.AddPoint(-98.65, 12.65);

GeometryCollection geometryCollection = new GeometryCollection();
geometryCollection.Add(point);
geometryCollection.Add(line);
```

## Typed geometry collections
There are special collections that only accept the corresponding kind of geometry element: <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/multilinestring">MultiLineString</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/multipoint">MultiPoint</a> and <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/multipolygon">MultiPolygon</a>. These only accept LineString, Point and Polygon accordingly:

```csharp
MultiPoint multipoint = new MultiPoint();
multipoint.Add(new Point(1, 2));
multipoint.Add(new Point(3, 4));

            LineString firstLine = new LineString();
firstLine.AddPoint(7.5, -3.5);
firstLine.AddPoint(-9.6, 12.6);

LineString secondLine = new LineString();
secondLine.AddPoint(8.5, -2.6);
secondLine.AddPoint(-8.6, 1.5);

MultiLineString multiLineString = new MultiLineString();
multiLineString.Add(firstLine);
multiLineString.Add(secondLine);

            LinearRing firstRing = new LinearRing();
firstRing.AddPoint(8.5, -2.5);
firstRing.AddPoint(-8.5, 2.5);
firstRing.AddPoint(8.5, -2.5);
Polygon firstPolygon = new Polygon(firstRing);

LinearRing secondRing = new LinearRing();
secondRing.AddPoint(7.6, -3.6);
secondRing.AddPoint(-9.6, 1.5);
secondRing.AddPoint(7.6, -3.6);
Polygon secondPolygon = new Polygon(secondRing);

MultiPolygon multiPolygon = new MultiPolygon();
multiPolygon.Add(firstPolygon);
multiPolygon.Add(secondPolygon);
```

As you can see, creating geometry is easy.

## Geometry validation
Validation checks wether geometric data in an geometry instance is correct for the type of that instance, i.e. a polygon has an external boundary, a LinearRing is enclosed, etc. It is performed automatically and result is availible at <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/isvalid">IsValid</a> property. Take a look:
```csharp 
LinearRing linearRing = new LinearRing();
linearRing.AddPoint(0, 0);
linearRing.AddPoint(0, 1);
linearRing.AddPoint(1, 0);
bool valid = linearRing.IsValid; // valid == false , as ring is not enclosed
linearRing.AddPoint(0, 0);
valid = linearRing.IsValid; // valid == true, as ring is now enclosed
```
Additionally, there is a check for simplicity, in terms of <a href="http://www.opengeospatial.org/standards/sfa"SFA (Simple Feature Access)</a> - like no self-intersections, etc. It works same way as validation and availible at <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/properties/issimple">IsSimple</a> property.:
```csharp
            LineString lineString = new LineString();
            bool simple = lineString.IsSimple; // simple == true
            lineString.AddPoint(0, 0);
            lineString.AddPoint(1, 0);
            simple = lineString.IsSimple; // simple == true
            lineString.AddPoint(0.5, 0);
            simple = lineString.IsSimple; // simple == false (line string crosses itself)
```

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-gis">Aspose.GIS GitHub</a> page. There's also <a href="https://twitter.com/AsposeGis">Twitter</a> and <a href="https://www.facebook.com/AsposeGis/">Facebook</a> pages for news on Aspose.GIS.
