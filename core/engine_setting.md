# Adding/Editing Engine Settings
## Conventional Method
In KiriKiri 2, the core engine obtained a "|" delimited string (other delimiters were also used) via TVPGetCommandDesc and displayed engine setting items based on that.  
Additionally, for plugins, "--has-option--" was placed in the comment section, the GetOptionDesc function was exposed, and setting details were included within it using the same string format as the core engine.  
Although "--has-option--" was added to the linker via /COMMENT:, recent versions of Visual Studio ignore this, making it impossible to include it in the DLL binary using the standard method.

## Method from KiriKiri Z onwards
In KiriKiri Z, the list of setting items was changed to JSON format and moved from being embedded in the source code to being included in resources.  
The specific JSON format should be mostly understandable by looking at the file.  
For the core engine, editing the option_desc_ja.json resource is sufficient for changes to take effect, but the plugin side requires a bit of caution.  
The resource type is TEXT, and it is stored in the DLL with the string ID IDR_OPTION_DESC_JSON.  

Specifically, add it to the resource by writing the following in the *.rc file:  
IDR_OPTION_DESC_JSON TEXT "option_desc_ja.json"

Do not define IDR_OPTION_DESC_JSON in resource.h.  
If IDR_OPTION_DESC_JSON is defined, the resource ID will be added as a numeric value.

### JSON Format
Not written yet...