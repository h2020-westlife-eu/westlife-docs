# Embedding Virtual Folder Component

You can include some of the components of West-Life Virtual Folder in your application, e.g. using `<div>` tag:

```markup
<div aurelia-app="[componentname]">
  <script type="text/javascript" src="https://portal.west-life.eu/virtualfolder/app.bundle.js"></script>
  <script type="text/javascript" src="https://portal.west-life.eu/virtualfolder/vendor.bundle.js"></script>
  Loading ...
</div>
```

The `[componentname]` could be one of the following:

* `filepicker/main` - File picker - allows to pick the file from Virtual Folder and it's WEBDAV URI is returned to managing page. See the demo at filepicker.html and filepickercomponent.html.
* `filemanager/main` - File manager - allows to browse files in defined file providers \(demo view of PDB \). See the demo at filemanager.html.
* `dataset/main` - Dataset definition page - allows to define list of entries - PDB and Uniprot entries can be refined with
* `virtualfoldersettings/main` - Virtual Folder Settings page - allows to manage file providers \(B2DROP,Dropbox,Filesystem,...\)

The complete example of embedded component in your web may look:

```markup
<html>
<body>
<p>filemanager from West-Life VF</p>
<div aurelia-app="filemanager/main">
<script type="text/javascript" src="https://portal.west-life.eu/virtualfolder/app.bundle.js"></script>
<script type="text/javascript" src="https://portal.west-life.eu/virtualfolder/vendor.bundle.js"></script>
  Loading ...
</div>
</body>
</html>
```

## Flask integration example

Several web services provided within West-Life relies on [Flask python microframework](http://flask.pocoo.org/). Below are few lines allowing to handle the URL sent by the VF component. In this implementation, we assume that the Flask-wtf form, instead of taking a file as input, will deal with a `str` that is the URL sent by the VF component. The implementation example can be found at this URL \(only for DEVELOPMENT purposes, do not advertise it!!!\) -&gt; [https://milou.science.uu.nl/cgi/servicesdevel/DISVIS/disvis/submit](https://milou.science.uu.nl/cgi/servicesdevel/DISVIS/disvis/submit) The field is declared in the `forms.py`:

```python
pdb_url = TextField(
    u'OR select fixed chain from your VRE',
    render_kw={'class': 'form-control', 'placeholder': 'File URL once selected'},
    validators=[validators.Optional(), InvalidPDB_url()])
```

And in `submit.html`:

JavaScript

```javascript
$(document).ready(function(){
  $('label[for="pdb_url"]').append('<br><a href="javascript:void(0)"  onclick="openwindow(); return false;"> Choose VRE file.</a>')
}
// This does nothing, assuming the window hasn't changed its location.
function openwindow() {
    target = event.target.parentNode.getAttribute('for');
    popup=window.open('https://portal.west-life.eu/virtualfolder/filepickercomponent.html', 'newwindow', 'width=640, height=480');
}

function receiveMessage(event)
{
    if (event.data != "") {
        document.getElementById(target).value=event.data;
        $('#'+target).trigger("change");
    }
    //popup.close(); //closed by child
}
window.addEventListener("message", receiveMessage, false);
```

HTML

```markup
<form action="{{ url_for('submit') }}" method="POST" enctype="multipart/form-data">
{{ render_field(form.pdb_url) }}
</form>
```

Upon successful validation, we want to download the file to process them afterwards, in `views.py`:

```python
import urllib2

# Utility method to download a file locally from a URL
def download_file(file_url, work_dir, fname):
    logging.info("VF URL: {}".format(file_url))
    try:
        u = urllib2.urlopen(file_url)
        f = open(safe_join(work_dir, fname), 'wb')
        block_sz = 8192
        while True:
            buffer = u.read(block_sz)
            if not buffer:
                break
            f.write(buffer)
        f.close()
    except HTTPError:
        logging.error("Error during file download, aborting...")
        raise

@app.route('/submit', methods=['GET', 'POST'])
def submit():
form = InputDataForm()
... # Initialization of different variables..
...
if form.validate_on_submit():
  try:
    # Get URL from the TextField
    pdb_url = form['pdb_url'].data # Get 
    download_file(pdb_url, work_dir, fname)
    print os.path.isfile(safe_join(work_dir, fname)) # Should return 'True'
```

