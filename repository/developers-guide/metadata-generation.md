# Metadata generation

Metadata are generated on user's request. The Rest API is available for 

```text
GET /dataset/{datasetId}/metadata
```

Will return metadata in JSON format associated to the dataset.

```text
POST /dataset/{datasetId}/metadata
```

If the POST request is with empty body, then metadata are generated from files included in dataset. If POST request is with non-empty body, this body is used to store dataset metadata \(no operation is invoked to generate metadata from files\)

Metadata are stored in MongoDB instance. Implementation of getting and creating/updating metadata associated to dataset `id` is at `org.cirmmp.spring.metadata.MetadataDB` class.

In order to harvest metadata from different files, there are common classes in package `org.cirmmp.spring.metadata`:

*  `MetadataGenerator` with `harvestMetadata()` method which traverses over all files in dataset directory
* interface `AMetadataExtractor` with abstract `harvestFile()` method
* default extractor `DefaultMetadataExtractor` which can handle JCAMPDX format, CBF format or text with binary format containing metadata in form of `key = value` or `key: values` or `value1 value2 value3 ...`

### Adding specific extractor for metadata 

You need to implement interface `AMetadataExtractor` 

```java
import org.json.JSONObject;
...
public class NewExifMetadataExtractor implements AMetadataExtractor{
  public JSONObject harvestFile(Path file) throws IOException, FileNotFoundException{
    JSONObject meta = new JSONObject();
    meta.put("filename",fileordir.getFileName().toString())
    meta.put("manufacturer", getExifManufacturer());
    meta.put("model", getExifModel());
  }
  private String getExifManufacturer() {
   //my code here
  }
  private String getExifModel() {
  //my code here
  }
}
```

and register the file extension in static part of MetadataGenerator. Each file extension needs separate line. E.g.we register "png" and "jpg" file extension with thes new metadata extractor as follows:

```java
    static {
        type2extractor = new HashMap<String,Class>();
        type2extractor.put("txt",TxtMetadataExtractor.class);
        type2extractor.put("",DefaultMetadataExtractor.class);
        //new extractor here
        type2extractor.put("png",NewExifMetadataExtractor.class);
        type2extractor.put("jpg",NewExifMetadataExtractor.class);
}
```



