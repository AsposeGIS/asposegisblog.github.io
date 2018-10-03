---
layout: post
title: Introduction to Aspose.GIS
---

<a href="https://products.aspose.com/gis">Aspose.GIS</a> is a software library for .NET platforms that does all the common tasks related to geospatial data files: it converts between supported data formats, can convert geospatial data between different spatial reference systems, even allowing use of a custom reference system, provides convenient API for reading and editing of geospatial data and its attributes from files and also for creation of geospatial data from scratch. More than that, Aspose.GIS can perform some basic analysis of geospatial data, such as determining intersections and overlaps of geometric entities and validating geospatial data.
Here I will show basic examples on how to use these functions of Aspose.GIS. Later, I will explore these features deeper in separate articles.

## Concept of working with Aspose.GIS.
The main concept behind Aspose.GIS is use of a common intermediate representation for geospatial data and its attributes, independend of a particular data format used. This is a natural approach, as all the all GIS file formats represent the same kind of data. For example, a whole set of data from a GIS data file is represented as <a href="https://apireference.aspose.com/net/gis/aspose.gis/vectorlayer/">VectorLayer</a> class instance, which contains GIS features represented as Feature class instances, and so on. (GDB files contain many layers, they will be covered below.) This approach facilitates working with all the supported file formats the same way.

## Converting between different geodata formats.
Let's take a look on how you can convert data from one format to another. This operation is exceptionally simple, as there is a special static <a href="https://apireference.aspose.com/net/gis/aspose.gis/vectorlayer/methods/convert">Convert</a> method in VectorLayer class, which takes input file path, input file format, output file path and output file format. File formats are specifying by selecting a driver for corresponding file format, they are availible as properties of <a href="https://apireference.aspose.com/net/gis/aspose.gis/drivers/">Drivers</a> class. Currently, there are <a href="https://apireference.aspose.com/net/gis/aspose.gis/drivers/properties/geojson">GeoJson</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis/drivers/properties/kml">Kml</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis/drivers/properties/shapefile">Shapefile</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis/drivers/properties/osmxml">OsmXml</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis/drivers/properties/gpx">Gpx</a>, <a href="https://apireference.aspose.com/net/gis/aspose.gis/drivers/properties/gml">Gml</a> and <a href="https://apireference.aspose.com/net/gis/aspose.gis/drivers/properties/filegdb">FileGdb</a> drivers for corresponding file formats. Optionally, you can specify options as an instance of ConversionOptions, which provides control over attribute conversion, destination spatial reference system, and input and destination file format processing. Take a look at example:
```csharp
public void ConvertFormats()
{
    ConversionOptions options = new ConversionOptions();
	
	//Convert shapefile to GeoJSON
	VectorLayer.Convert("input.shp", Drivers.Shapefile, "output_out.json", Drivers.GeoJson);

	//Now let's try to change attributes during conversion and specify custom attribute converter
    options.AttributesConverter = new AttributesConverterExample();
    
	//Convert GeoJSON to shapefile using ConversionOptions with custom converter
    VectorLayer.Convert("input.json", Drivers.GeoJson,
	"ConvertGeoJSONToShapeFileWithAttributeAdjustment_out.shp", Drivers.Shapefile, options);
}

private class AttributesConverterExample : IAttributesConverter
{
	//This will simply be called for every attribute in the input file, allowing you to modify the attribute.
    public void ModifyAttribute(FeatureAttribute attribute)
    {
        switch (attribute.Name)
        {
            case "name":
                attribute.Width = 10;
                break;
            case "age":
                attribute.Width = 3;
                attribute.Precision = 0;
                break;
        }
    }
}
```

## Working with Spatial Reference Systems
Spatial reference systems are represented as instances of <a href="https://apireference.aspose.com/net/gis/aspose.gis.spatialreferencing/spatialreferencesystem">SpatialReferenceSystem</a> class, and are also managed via static methods of this class. Common SRS are availible as properties of SpatialReferenceSystem class, you can also create your own either by providing WKT description or by instantiating  <a href="https://apireference.aspose.com/net/gis/aspose.gis.spatialreferencing/projectedspatialreferencesystemparameters">ProjectedSpatialReferenceSystemParameters</a> class. Take a look at the examples:

#### Create an instance of SRS from WKT and compare it to an prefefined WGS84 SRS:
```csharp
            string wkt = @"
GEOGCS[""WGS 84"",
    DATUM[""WGS_1984"",
        SPHEROID[""WGS 84"",6378137,298.257223563,
            AUTHORITY[""EPSG"",""7030""]],
        AUTHORITY[""EPSG"",""6326""]],
    PRIMEM[""Greenwich"",0,
        AUTHORITY[""EPSG"",""8901""]],
    UNIT[""degree"",0.01745329251994328,
        AUTHORITY[""EPSG"",""9122""]],
    AUTHORITY[""EPSG"",""4326""]]
";
            var srs = SpatialReferenceSystem.CreateFromWkt(wkt);
            srs.IsEquivalent(SpatialReferenceSystem.Wgs84); // true
```

#### Create an SRS via API and export to WKT
```csharp
var parameters = new ProjectedSpatialReferenceSystemParameters
{
    Name = "WGS 84 / World Mercator",
    Base = SpatialReferenceSystem.Wgs84,
    ProjectionMethodName = "Mercator_1SP",
    LinearUnit = Unit.Meter,
    XAxis = new Axis("Easting", AxisDirection.East),
    YAxis = new Axis("Northing", AxisDirection.North),
    AxisesOrder = ProjectedAxisesOrder.XY,
};
parameters.AddProjectionParameter("central_meridian", 0);
parameters.AddProjectionParameter("scale_factor", 1);
parameters.AddProjectionParameter("false_easting", 100);
parameters.AddProjectionParameter("false_northing", 100);

var projectedSrs = SpatialReferenceSystem.CreateProjected(parameters, Identifier.Epsg(3395));

string wkt = projectedSrs.ExportToWkt();
Console.WriteLine(wkt);
```

#### Check for support of particular SRS in file format
```csharp
Drivers.Shapefile.SupportsSpatialReferenceSystem(SpatialReferenceSystem.Wgs72); // true
Drivers.GeoJson.SupportsSpatialReferenceSystem(SpatialReferenceSystem.Wgs84); // true
Drivers.GeoJson.SupportsSpatialReferenceSystem(SpatialReferenceSystem.Wgs72); // false
```

As you can see, working with SRS in Aspose.GIS is straightforward.

## Reading features from geodata files
Files are loaded as instances of VectorLayer class. There are two approaches to loading files: either call <a href="https://apireference.aspose.com/net/gis/aspose.gis/driver/methods/openlayer">OpenLayer</a> on a static property of Drivers class that corresponds to your file format, specifying path to file, or call static <a href="https://apireference.aspose.com/net/gis/aspose.gis/vectorlayer/methods/open">Open</a> method of VectorLayer class, specifying path to file and a corresponding file format driver (again, via static property of Drivers class). Example here shows first approach.
Geospatial features are availible right by iteration over VectorLayer instance. Geometric data is stored at <a href="https://apireference.aspose.com/net/gis/aspose.gis/feature/properties/geometry">Geometry</a> property of <a href="https://apireference.aspose.com/net/gis/aspose.gis/feature/">Feature</a>, and attributes can be read using <a href="https://apireference.aspose.com/net/gis/aspose.gis/feature/methods/getvalue/_1">GetValue<T>()</a> method:
```csharp
using (var layer = Drivers.OsmXml.OpenLayer(dataDir + "fountain.osm"))
{
    // get feratures count
    int count = layer.Count;

    Console.WriteLine("Layer count: " + count);
    // get feature at index 2
    Feature featureAtIndex2 = layer[2];

    // iterate through all features.
    foreach (Feature feature in layer)
    {
        // handle feature
        Console.WriteLine(feature.Geometry.GeometryType);
		
		//read attribute and display if it's present
		string data = featureAtIndex2.GetValue<string>("string_data");
		if(data!=null)
			Console.WriteLine(data);
    }
}
```


#### Working with multi-layer GDB files
As GDB files contain many layers, they have to be handled differently. They are represented as instances of <a href="https://apireference.aspose.com/net/gis/aspose.gis/dataset">Dataset</a> class, and loading process is similar to VectorLayer: call static <a href="https://apireference.aspose.com/net/gis/aspose.gis/dataset/methods/open">Open</a> method of Dataset providing path to file and file format driver, or instead call <a href="https://apireference.aspose.com/net/gis/aspose.gis/driver/methods/opendataset">OpenDataset</a> method of a <a href="https://apireference.aspose.com/net/gis/aspose.gis/driver">Driver</a>. Currently only GDB driver works with Dataset.Open, but it has to be specified for uniformity. Then, separate layers can be picked by using <a href="https://apireference.aspose.com/net/gis/aspose.gis/dataset/methods/openlayerat">OpenLayerAt</a> method, which takes layer index. Total count of layers is stored in <a href="https://apireference.aspose.com/net/gis/aspose.gis/dataset/properties/layerscount">LayersCount</a> property. Layers can also be loaded by name using <a href="https://apireference.aspose.com/net/gis/aspose.gis/dataset/methods/openlayer">OpenLayer</a> method.
```csharp
using (var dataset = Dataset.Open(dataDir + "ThreeLayers.gdb", Drivers.FileGdb))
{
    Console.WriteLine("FileGDB has {0} layers", dataset.LayersCount);
    for (int i = 0; i < dataset.LayersCount; ++i)
    {
        Console.WriteLine("Layer {0} name: {1}", i, dataset.GetLayerName(i));

        using (var layer = dataset.OpenLayerAt(i))
        {
            Console.WriteLine("Layer has {0} features", layer.Count);
            foreach (var feature in layer)
            {
                Console.WriteLine(feature.Geometry);
            }
        }
        Console.WriteLine("");
    }
}
```


## Creating a new geodata file and filling it with data
Creation is done in a way similar to opening an existing file, just using <a href="https://apireference.aspose.com/net/gis/aspose.gis/vectorlayer/methods/create">Create()</a> method instead of Open(). After creation, you have an VectorLayer instance that you can add attributes and Feature(s) to. When you dispose the object, changes will be pushed to file.
```csharp
using (VectorLayer layer = VectorLayer.Create("ShapeFile.shp", Drivers.Shapefile))
{
    // add attributes before adding features
    layer.Attributes.Add(new FeatureAttribute("name", AttributeDataType.String));
    layer.Attributes.Add(new FeatureAttribute("age", AttributeDataType.Integer));
    layer.Attributes.Add(new FeatureAttribute("dob", AttributeDataType.DateTime));

    Feature firstFeature = layer.ConstructFeature();
    firstFeature.Geometry = new Point(33.97, -118.25);
    firstFeature.SetValue("name", "John");
    firstFeature.SetValue("age", 23);
    firstFeature.SetValue("dob", new DateTime(1982, 2,5, 16, 30,0));
    layer.Add(firstFeature);

    Feature secondFeature = layer.ConstructFeature();
    secondFeature.Geometry = new Point(35.81, -96.28);
    secondFeature.SetValue("name", "Mary");
    secondFeature.SetValue("age", 54);
    secondFeature.SetValue("dob", new DateTime(1984, 12, 15, 15, 30, 0));
    layer.Add(secondFeature);
}
```

## Basic analysis: checking for intersections
Geometry class provides instance methods for basic geometry analysis. In this example I'll show use of <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/intersects">Intersects()</a> and <a href="https://apireference.aspose.com/net/gis/aspose.gis.geometries/geometry/methods/disjoint">Disjoint()</a> methods, but there are also other methods.
```csharp
//Create geometry first.
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

### Supported file formats
Aspose.GIS can read and create GeoJSON files, ESRI Shapefiles and File Geodatabases, Keyhole Markup Language (KML) files, OpenStreetMap XML (OSM XML) files, Geography Markup Language (GML) files, and it can also read GPS Exchange Format (GPX) files. 

### Supported platforms
Aspose.GIS is availible for .NET Framework 4.7 and later, and for .NET Standard 2.0 and later. As such, it works on such platforms as .NET Framework, .NET Core, Mono 5.4 or later, Xamarin.iOS 10.14 or later, Xamarin.Mac 3.8 or later and Xamarin.Android 8.0 or later.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-gis">Aspose.GIS GitHub</a> page. There's also <a href="https://twitter.com/AsposeGis">Twitter</a> and <a href="https://www.facebook.com/AsposeGis/">Facebook</a> pages for news on Aspose.GIS.
