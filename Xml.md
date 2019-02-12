## processContents
Specifies how the XML processor should handle validation against the elements specified by this any element. Can be set to one of the following:
- **strict** - the XML processor must obtain the schema for the required namespaces and validate the elements (this is default)
- **lax** - same as strict but; if the schema cannot be obtained, no errors will occur
- **skip** - The XML processor does not attempt to validate any elements from the specified namespaces