---
layout: post
title: Magic Numbers
comments: true
---
So recently I needed to determine file types by their magic numbers.  Now I could have done the more sensible thing and 
just used a python library like [python filemagic](https://filemagic.readthedocs.org/en/latest/) which relies on lib magic.  
Which has very well layout and defined output and would be the most preferable way for detecting file types.  With that 
being said though, learning a little bit about magic numbers and how file types can be detected in python was a good 
exercise and it fit a very specific need.

The basic idea for detecting magic numbers of a file is to read a specific amount of bytes and look at the file headers 
or identifiers for specific bits.  For example, a zip file has a header of these bytes: \x50\x4B\x03\x04 so if they 
appear at the head of the file then it is a type zip.  Pretty basic really at this point.  Where things get complicated 
is that with zip files especially is that the definition of what makes a zip is fairly loose for one and several other 
types of files are also technically zip files.  For example Microsoft Word docx format is detected as a zip file can 
even be extracted as a zip file.  In our particular use that was not really the desired behaviour.  The problem with 
detecting these files is the detection is not perfect unless decompressing the file and checking the contents of the 
archive.  That also has a trade off, it may not be desirable or feasible to unpack every archive into memory to 
determine the actual identity of the file.  So in this regard we kind of cheat and just fall back to relying on the 
extension name.  This is not a perfect detection of the file but it generally will do a fair job in a pinch.  There are 
other ways of doing this and making it more exact, there are header pieces that can be checked to determine if the file 
is an open xml format (both word and open office).  Really though this is probably best handled with python file magic 
and this was an interesting exercise in identifying file types based on magic numbers.

```python
MAGIC_NUMBERS = {
    # List of magic numbers to determine file types.
    # A partial Listing is available at: http://en.wikipedia.org/wiki/List_of_file_signatures
    'zip': {'numbers': ['\x50\x4B\x03\x04'], 'offset': 0},
    'gz': {'numbers': ['\x1F\x8B\x08'], 'offset': 0},
    'bz2': {'numbers': ['\x42\x5A\x68'], 'offset': 0},
    'tar': {'numbers': ['\x75\x73\x74\x61\x72\x00\x30\x30', '\x75\x73\x74\x61\x72\x20\x20\x00'], 'offset': 257},
    'rar': {'numbers': ['\x52\x61\x72\x21\x1A\x07\x00', '\x52\x61\x72\x21\x1A\x07\x01\x00'], 'offset': 0},
    '7z': {'numbers': ['\x37\x7A\xBC\xAF\x27\x1C'], 'offset': 0},
    'Z': {'numbers': ['\x1F\x9D'], 'offset': 0}
}


def determine_filetype(target_file):
    """
    Reads the headers of a file and determines the file type based on the headers.
    :param target_file: File to check what the file type is
    :return: Short name of the type of file, like gz for gzipped archives, bz2 for bzipped archives. Doesn't make any
             inferences to what is contained in the file in cases of archives, for example a tar.gz file will return
             that it is a gzip archive, but won't know that there is a tar inside of it. Possible Returns: False,
             apk, docx, jar, odp, ods, odt, pptx, xlsx, zip, gz, bz2, tar, rar, 7z, Z (as configured in MAGIC_NUMBERS)
    """
    if not os.path.isfile(target_file):
        # Not a regular file, don't bother.
        return False
   
    alternate_zips = ['apk', 'docx', 'jar', 'odp', 'ods', 'odt', 'pptx', 'xlsx', 'zipx']
    magic_number_lengths = []
    header_offsets = []
    for file_type in MAGIC_NUMBERS:
        header_offsets.append(MAGIC_NUMBERS[file_type]['offset'])
        for number in MAGIC_NUMBERS[file_type]['numbers']:
            magic_number_lengths.append(len(number))

    header_length = max(magic_number_lengths) + max(header_offsets)

    with open(target_file) as raw_file:
        headers = raw_file.read(header_length)

    for file_type in MAGIC_NUMBERS:
        for magic in MAGIC_NUMBERS[file_type]['numbers']:
            if headers[MAGIC_NUMBERS[file_type]['offset']:].startswith(magic):
                if file_type == 'zip':
                    file_extension = os.path.splitext(target_file)[1][1:]
                    if file_extension in alternate_zips:
                        return file_extension
                return file_type
return False # No filetypes matched.
```