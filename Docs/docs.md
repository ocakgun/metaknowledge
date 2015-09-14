---
layout: page
title: metaknowledge Docs
---
<a name="metaknowledge"></a>Doc String for metaknowledge main

## Classes

<a name="metaknowledge.BadCitation"></a>metaknowledge.**BadCitation**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Exception thrown by Citation


- - -

<a name="metaknowledge.BadISIRecord"></a>metaknowledge.**BadISIRecord**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Exception thrown by the [record parser](#metaknowledge.recordParser) to indicate a mis-formated record. This occurs when some component of the record does not parse. The messages will be any of:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    * _Missing field on line (line Number):(line)_, which indicates a line was to short, there should have been a tag followed by information

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    * _End of file reached before ER_, which indicates the file ended before the 'ER' indicator appeared, 'ER' indicates the end of a record. This is often due to a copy and paste error.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    * _Duplicate tags in record_, which indicates the record had 2 or more lines with the same tag.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    * _Missing WOS number_, which indicates the record did not have a 'UT' tag.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Records with a BadISIRecord error are likely incomplete or the combination of two or more single records.


- - -

<a name="metaknowledge.Citation"></a>metaknowledge.**Citation**(_cite_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A class to hold citation strings and allow for comparison between them.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The initializer takes in a string representing a WOS citation they are in the form:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Author, Year, Journal, Volume, Page, DOI

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Author is the author's name in the form of first last name first initial sometimes followed by a period.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Year is the year of publication.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Journal being the 29-Character Source Abbreviation of the journal.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Volume is the volume number(s) of the publication preceded by a V
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Page is the page number the record starts on
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DOI is the DOI number of the cited record preceeded by the letters "DOI"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Combined they look like:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Nunez R., 1998, MATH COGNITION, V4, P85, DOI 10.1080/135467998387343

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Note that any of the fields have been known to be missing and the requirements for the fields are not always met. If something is in the source string that cannot be interpeted as any of these it is put in the `misc` attribute.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The reason for this class is that the WOS data are often irregular. It is designed to allow comparison between WOS citation strings, even when they are missing pieces.

#####&nbsp;&nbsp;&nbsp; Customizations

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Citation's hashing and equality checking are based on what data they have. The equality checking first checks both Citation's DOI's and if either is missing moves to the other fields. If any of the fields disagree `False` is returned (note, authors are not compared if one is anonymous) if they all agree, including the `misc` field, then True is returned.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unfortunately this type of equality checking precludes hashes being identical so to compare Citation objects always use ==. Hashes, if identical, indicate the Citations are identical (excluding collisions), but the converse is not True.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When converted to a string a Citation will return the original string.

#####&nbsp;&nbsp;&nbsp; Attributes

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;As noted above, citations are considered to be divided into six distinct fields (Author, Year, Journal, Volume, Page and DOI) with a seventh misc for anything not in those. Records thus have an attribute with a name corresponding to each `author`, `year`, `journal`, `V`, `P`, `DOI` and `misc` respectively. These are created if there is anything in the field. So a Citation created from the string: "Nunez R., 1998, MATH COGNITION" would have `author`, `year` and `journal` defined. While one from "Nunez R." would have only the attribute `misc`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If the parsing of a citation string fails the attribute `bad` is set to True and the attribute `error` is created to contain the error, which is a [BadCitation](#metaknowledge.BadCitation) object. If no errors occur `bad` is `False`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The attribute `original` is the unmodified string (_cite_) given to create the Citation, it can also be accessed by converting to a string, e.g. with `str()`.

#####&nbsp;&nbsp;&nbsp; \_\_Init\_\_

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Citations can be created by [Records](#metaknowledge.Record) or by giving the initializer a string containing a WOS style citation.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_cite_ : `str`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A str containing a WOS style citation.


<a name="Citation.addToDB"></a>Citation.**addToDB**(_manualName=None, manaulDB='manualj9Abbreviations', invert=False_):

# Needs to be written

<a name="Citation.getExtra"></a>Citation.**getExtra**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns any V, P or misc values as a string. These are all the values not returned by [`getID()`](#Citation.getID).

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`str`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A string containing the data not in the ID of the Citation.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 


<a name="Citation.getFullJournalName"></a>Citation.**getFullJournalName**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns the full name of the Citation's journal field. Requires the j9Abbreviations database file.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`str`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The first full name given for the journal of the Citation (or the first name in the WOS list if multiple names exist), if there is not one then `None` is returned


<a name="Citation.getID"></a>Citation.**getID**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns "author, year, journal" if available and "misc" otherwise. It is for shortening labels when creating networks as the resultant strings are often unique. [`getExtra()`](#Citation.getExtra) gets everthing not returned by `getID()`.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`str`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A string to use as the shortened ID of a node.


<a name="Citation.isAnonymous"></a>Citation.**isAnonymous**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Checks if the author is given as "[ANONYMOUS]" and returns `True` if so.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`bool`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; True if the author is ANONYMOUS otherwise `False`.


<a name="Citation.isJournal"></a>Citation.**isJournal**(_manaulDB='manualj9Abbreviations', returnDict='both', checkIfExcluded=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns `True` if the Citation's journal field is a journal abbreviation given by WOS, i.e. checks if the citation is citing a journal. Requires the j9Abbreviations database file.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`bool`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `True` if the Citation is for a journal


- - -

<a name="metaknowledge.Record"></a>metaknowledge.**Record**(_inRecord, taglist=(), sFile='', sLine=0_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Class for full WOS records

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;It is meant to be immutable; many of the methods and attributes are evaluated when first called, not when the object is created, and the results are stored in a private dictionary.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The record's meta-data is stored in an ordered dictionary labeled by WOS tags. To access the raw data stored in the original record the [getTag()](#Record.getTag) method can be used. To access data that has been processed and cleaned the attributes named after the tags are used.

#####&nbsp;&nbsp;&nbsp; Customizations

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The Record's hashing and equality testing are based on the WOS number (the tag is 'UT', and also called the accession number). They are strings starting with "WOS:" and followed by 15 or so numbers and letters, although both the length and character set are known to vary. The numbers are unique to each record so are used for comparisons. If a record is `bad`  all equality checks return `False`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When converted to a string the records title is used so for a record `R`, R.TI == R.title == str(R).

#####&nbsp;&nbsp;&nbsp; Attributes

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When a record is created if the parsing of the WOS file failed it is marked as `bad`. The `bad` attribute is set to True and the `error` attribute is created to contain the exception object.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Generally, to get the information from a Record its attributes should be used. For a Record `R`, calling `R.CR` causes [citations()](#metaknowledge.tagFuncs.citations) from the the [tagFuncs](#metaknowledge.tagFuncs) module to be called on the contents of the raw 'CR' field. Then the result is saved and returned. In this case, a list of Citation objects is returned. You can also call `R.citations` to get the same effect, as each known field tag has a longer name (currently there are 61 field tags). These names are meant to make accessing tags more readable and mapping from tag to name can be found in the tagToFull dict. If a tag is known (in [tagToFull](#metaknowledge)) but not in the raw data `None` is returned instead. Most tags when cleaned return a string or list of strings, the exact results can be found in the help for the particular function.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The attribute `authors` is also defined as a convience and returns the same as 'AF' or if that is not found 'AU'.

#####&nbsp;&nbsp;&nbsp; \_\_Init\_\_

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Records are generally create as collections in  [Recordcollections](#metaknowledge.RecordCollection), and not as individual objects. If you wish to create one on its own it is possible, the arguments are as follows.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_inRecord_: `files stream, dict, str or itertools.chain`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; If it is a file stream the file must be open at the location of the first tag in the record, usually 'PT', and the file will be read until 'ER' is found, which indicates the end of the record in the file.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; If a dict is passed the dictionary is used as the database of fields and tags, so each key is considered a WOS tag and each value a list of the lines of the original associated with the tag. This is the same form of dict that [recordParser](#metaknowledge.recordParser) returns.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; For a str the input is the raw textual data of a single record in the WOS style, like the file stream it must start at the first tag and end in 'ER'.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itertools.chain is treated identically to a file stream and is used by [RecordCollections](#metaknowledge.RecordCollection).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_sFile_ : `optional [str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Is the name of the file the raw data was in, by default it is blank. It is mostly used to make error messages more informative.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_sLine_ : `optional [int]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Is the line the record starts on in the raw data file. It is mostly used to make error messages more informative.


<a name="Record.activeTags"></a>Record.**activeTags**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a list of all the tags the original WOS record had. These are all the tags that ['getTag()'](#Record.getTag) will not return `None` for.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`List[str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a list of WOS tags in the Record


<a name="Record.createCitation"></a>Record.**createCitation**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Creates a citation string, using the same format as other WOS citations, for the [Record](#metaknowledge.Record) by reading the relevant tags (year, J9, volume, beginningPage, DOI) and using it to start a [Citation](#metaknowledge.Citation) object.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`Citation`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A [Citation](#metaknowledge.Citation) object containing a citation for the Record.


<a name="Record.getTag"></a>Record.**getTag**(_tag, clean=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a list containing the raw data of the record associated with _tag_. Each line of the record is one string in the list.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_tag_ : `str`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _tag_ can be a two character string corresponding to a WOS tag e.g. 'J9', the matching is case insensitive so 'j9' is the same as 'J9'. Or it can be one of the full names for a tag with the mappings in [fullToTag](#metaknowledge). If the string is not found in the original record or after being translated through [fullToTag](#metaknowledge), `None` is returned.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`List [str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Each string in the list is a line from the record associated with _tag_ or None if not found.


<a name="Record.getTagsDict"></a>Record.**getTagsDict**(_taglst, cleaned=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a dict of the results of getTag, with the elements of _taglst_ as the keys and the results as the values.

#####&nbsp;&nbsp;&nbsp; Parameters
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_taglst_ : `List[str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Each string in _taglst_ can be a two character string corresponding to a WOS tag e.g. 'J9', the matching is case insensitive so 'j9' is the same as 'J9'. Or it can be one of the full names for a tag with the mappings in [fullToTag](#metaknowledge). If the string is not found in the oriagnal record before or after being translated through [fullToTag](#metaknowledge), `None` is used instead. Same as in [`getTag()`](#Record.getTag)

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dict[str : List [str]]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a dictionary with keys as the original tags in _taglst_ and the values as the results


<a name="Record.getTagsList"></a>Record.**getTagsList**(_taglst, cleaned=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a list of the results of [`getTag()`](#Record.getTag) for each tag in _taglist_, the return has the same order as the original.

#####&nbsp;&nbsp;&nbsp; Parameters
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_taglst_ : `List[str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Each string in _taglst_ can be a two character string corresponding to a WOS tag e.g. 'J9', the matching is case insensitive so 'j9' is the same as 'J9'. Or it can be one of the full names for a tag with the mappings in [fullToTag](#metaknowledge). If the string is not found in the original record before or after being translated through [fullToTag](#metaknowledge), `None` is used instead. Same as in [`getTag()`](#Record.getTag)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Then they are compiled into a list in the same order as _taglst_

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`List[str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a list of the values for each tag in _taglst_, in the same order


<a name="Record.writeRecord"></a>Record.**writeRecord**(_infile_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Writes to _infile_ the original contents of the Record. This is intended for use by [RecordCollections](#metaknowledge.RecordCollection) to write to file. What is written to _infile_ is bit for bit identical to the original record file. No newline is inserted above the write but the last character is a newline.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_infile_ : `file stream`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; An open utf-8 encoded file


- - -

<a name="metaknowledge.RecordCollection"></a>metaknowledge.**RecordCollection**(_inCollection=None, name='', extension=''_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A way of containing a large number of Record objects, it provides ways of creating them from an isi file, string, list of records or directory containing isi files. The Records are containing within a set and as such many of the set operations are defined, pop, union, in ... also records are hashed with their WOS string so no duplication can occur.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The comparison operators <, <=, >, >= are based strictly on the number of Records within the collection, while equality looks for an exact match on the Records

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When being created if there are issues the Record collection will be declared bad (self.bad = True) it will then mostly return nothing or False. The error attribute contains the exception that occurred.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;They also possess a name accessed with repr(), this is used to auto generate the names of files and can be set at creation, note though that any operations that modify the RecordCollection's contents will update the name to include what occurred, read __repr__'s doc string for more information

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;inCollection is the object containing the information about the Records to be constructed it can be an isi file, string, list of records or directory containing isi files

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;name sets the name of the of the record if left blank name will be generated based on the object that created the Recordcollection

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;extension controls the extension that __init__ looks for when reading a directory, set it to the extension on the isi files you wish to load, if left blank all files will be tried and any that are not isi files will be silently skipped

#####&nbsp;&nbsp;&nbsp; \_\_Init\_\_

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RecordCollections are made from either a single file or directory supplied as _inCollection_.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_inCollection_ : `optional [str] or None`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the name of the source of WOS records. It can be skipped to produce an empty collection.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; If a file is provided. First it is checked to see if it is a WOS file (the header is checked). Then records are read from it one by one until the 'EF' string is found indicating the end of the file.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; If a directory is provided. First each file in the directory is checked for the correct header and all those that do are then read like indivual files. The records are then collected into a single set in the RecordCollection.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_name_ : `optional [str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The name of the RecordCollection, defaults to empty string. If left empty the name of the Record collection is set to the name of the file or directory used to create the collection. If provided the name id set to _name_

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_extension_ : `optional [str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The extension to search for when reading a directoy for files. _extension_ is the suffix searched for when a direcorty is read for files, by default it is empty so all files are read.


<a name="RecordCollection.citationNetwork"></a>RecordCollection.**citationNetwork**(_dropAnon=True, nodeType='full', extraInfo=False, weighted=True, dropNonJournals=False, saveJournalNames=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Creates a citation network for the RecordCollection.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_dropAnon_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `True`, if `True` citations labeled anonymous are removed from the network

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_authorship_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `False`, wether to use author's names as the node ID or the whole citations, if `True` names are used if `False` hashes are used

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_extraInfo_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `True`, wether the original citation string is added to the node as an extra value, if `True` it is

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_weighted_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `True`, wether the edges are weighted. If `True` the edges are weighted by the number of citations.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_dropNonJournals_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `False`, wether to drop ciations of non-journals

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_saveJournalNames_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `False`, wether to save the full name of the journals of citations. Citations missing a journal will have the string "None", use with _dropNonJournals_ to avoid this

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`Networkx DiGraph`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A networkx digraph with hashes as ID and citations as edges


<a name="RecordCollection.citeFilter"></a>RecordCollection.**citeFilter**(_keyString='', field='all', reverse=False, caseSensitive=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Filters Records by some string, keyString, in all of their citations.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns all Records with at least one citation possessing keyString in the field given by field.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;keyString give the string to be searched for if it is is blank then all citations with the specified field will be matched

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;field give the component of the citation to be looked at, it is one of a few strings. The default is 'all' which will cause the entire original citation to be searched. It can be used to search across fields, e.g. '1970, V2' is a valid keystring
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The other options are:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;author, searches the author field
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;year, searches the year field
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;journal, searches the journal field
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;V, searches the volume field
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;P, searches the page field
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;misc, searches all the remaining uncategorized information
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;anonymous, searches for anonymous citations, keyString is not used
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bad, searches for bad citations, keyString is not used

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;reverse being True causes all Records not matching the query to be returned, default is False

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;caseSensitive if True causes the search across the original to be case sensitive, only the 'all' option can be case sensitive


<a name="RecordCollection.coAuthNetwork"></a>RecordCollection.**coAuthNetwork**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Creates a coauthorship network for the RecordCollection.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`Networkx Graph`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A networkx graph with author names as nodes and collaborations as edges.


<a name="RecordCollection.coCiteNetwork"></a>RecordCollection.**coCiteNetwork**(_dropAnon=True, nodeType='full', nodeInfo=True, fullInfo=False, weighted=True, dropNonJournals=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Creates a co-citation network for the RecordCollection.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_dropAnon_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `True`, if `True` citations labeled anonymous are removed from the network

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_nodeType : `optional [str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `Full`, can alos be "original", "author", "journal" or "year"

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_extraInfo_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `True`, wether the original citation string is added to the node as an extra value, if `True` it is

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_weighted_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `True`, wether the edges are weighted. If `True` the edges are weighted by the number of occurrences of the co-citation.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_dropNonJournals_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `False`, wether to drop ciations of non-journals

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_saveJournalNames_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default `False`, wether to save the full name of the journals of citations. Citations missing a journal will have the string "None", use with _dropNonJournals_ to avoid this

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`Networkx Graph`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A networkx graph with hashes as ID and co-citation as edges


<a name="RecordCollection.dropBadRecords"></a>RecordCollection.**dropBadRecords**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Removes all Records with bad attributes == True from the collection


<a name="RecordCollection.dropNonJournals"></a>RecordCollection.**dropNonJournals**(_ptVal='J', dropBad=True, invert=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Drops the non journal type Records from the collection

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_ptVal_ : `optional [str]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The value of the PT tag to be kept, default is 'J' the journal tag

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_dropBad_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Determines if bad Records will be dropped as well, default `True`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_invert_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Set `True` to drop journals (or the PT tag given by _ptVal) instead of keeping them. Note, it still drops bad Records if _dropBad_ is `True`, default `False`


<a name="RecordCollection.dropWOS"></a>RecordCollection.**dropWOS**(_wosNum_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Removes the Record with WOS number (ID number) _wosNum_

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_wosNum_ : `str`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _wosNum_ is the WOS number of the Record to be dropped. _wosNum_ must begin with 'WOS:' or a valueError is raise.


<a name="RecordCollection.extractTagged"></a>RecordCollection.**extractTagged**(_taglist_):

# Needs to be written

<a name="RecordCollection.getBadRecords"></a>RecordCollection.**getBadRecords**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns RecordCollection containing all the Record which have their bad flag set to True, i.e. all those removed by dropBadRecords()


<a name="RecordCollection.getWOS"></a>RecordCollection.**getWOS**(_wosNum, drop=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Gets the Record from the collection by its WOS number.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_wosNum_ : `str`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _wosNum_ is the WOS number of the Record to be extracted. _wosNum_ must begin with 'WOS:' or a valueError is raise.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_drop_ : `optional [bool]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Default `False`. If `True` the Record is dropped from the collection after being extract, i.e. if `False` [getWOS()](#RecordCollection.getWOS) acts like [peak()](#RecordCollection.peak), if `True` it acts like [pop()](#RecordCollection.pop)

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`metaknowledge.Record`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The Record whose WOS number is _wosNum_


<a name="RecordCollection.makeDict"></a>RecordCollection.**makeDict**(_onlyTheseTags=None, longNames=False, cleanedVal=True_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a dict with each key a tag and the values being lists of the values for each of the Records in the collection, `None` is given when there is no value and they are in the same order across each tag.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When used in pandas: `pandas.DataFrame(RC.makeDict())` returns a data frame with each column a tag and each row a Record.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;See writeCSV()


<a name="RecordCollection.nModeNetwork"></a>RecordCollection.**nModeNetwork**(_tags, recordType=True, nodeCount=True, edgeWeight=True_):

# Needs to be written

<a name="RecordCollection.oneModeNetwork"></a>RecordCollection.**oneModeNetwork**(_mode, nodeCount=True, edgeWeight=True_):

# Needs to be written

<a name="RecordCollection.peak"></a>RecordCollection.**peak**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a random Record from the recordCollection, the Record is kept in the collection, use pop for faster destructive access


<a name="RecordCollection.pop"></a>RecordCollection.**pop**():

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a random Record from the recordCollection, the Record is deleted from the collection, use peak for nondestructive access


<a name="RecordCollection.twoModeNetwork"></a>RecordCollection.**twoModeNetwork**(_tag1, tag2, directed=False, recordType=True, nodeCount=True, edgeWeight=True_):

# Needs to be written

<a name="RecordCollection.writeCSV"></a>RecordCollection.**writeCSV**(_fname=None, onlyTheseTags=None, longNames=False, firstTags=['UT', 'PT', 'TI', 'AF', 'CR'], csvDelimiter=',', csvQuote='"', listDelimiter='|'_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Writes all the Records from the collection into a csv file with each row a record and each column a tag

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;fname is the name of the file to write to, if none is given it uses the Collections name suffixed by .csv

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;onlyTheseTags lets you specify which tags to use, if not given then all tags in the records are given.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If you want to use all known tags the use onlyTheseTags = metaknowledge.knownTagsList

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;longNames if set to True will convert the tags to their longer names, otherwise the short 2 character ones will be used

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;firstTags is the column's tags, it is set by default to ['UT', 'PT', 'TI', 'AF', 'CR'] so those will be written first if given by onlyTheseTags.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Note if tags are in firstTags but not in onlyTheseTags, onlyTheseTags will override onlyTheseTags

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;csvDelimiter is the delimiter used for the cells of the csv file, default is the comma (,)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;csvQuote is  the quote character used for the csv, default is the double quote (")

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;listDelimiter is the delimiter used between values of the same cell if the tag for that record has multiple outputs, default is the pipe (|)


<a name="RecordCollection.writeFile"></a>RecordCollection.**writeFile**(_fname=None_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Writes the RecordCollection to a file, the written file is identical to those download from WOS. The order of Records written is random.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;fname set the name of the file, if blank the RecordCollection's name's first 200 characters are use with the suffix .isi


<a name="RecordCollection.yearSplit"></a>RecordCollection.**yearSplit**(_startYear, endYear_):

# Needs to be written

## Functions

<a name="metaknowledge.blondel"></a>metaknowledge.**blondel**(_G, weightParameter=None, communityParameter='community'_):

# Needs to be written

- - -

<a name="metaknowledge.btest"></a>metaknowledge.**btest**(_quite=False_):

# Needs to be written

- - -

<a name="metaknowledge.drop_edges"></a>metaknowledge.**drop_edges**(_grph, minWeight=-inf, maxWeight=inf, parameterName='weight', ignoreUnweighted=False, dropSelfLoops=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a graph with edges whose weight is within the inclusive bounds of minWeight and maxWeight, i.e minWeight <= edges weight <= maxWeight, will throw a Keyerror if the graph is unweighted

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;minWeight and maxWeight default to negative and positive infinity respectively so without specifying either the output should be the input

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parameterName is key to weight field in the edge's dictionary, default is weight as that is almost always correct

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ignoreUnweighted can be set False to suppress the KeyError and make unweighted edges be ignored


- - -

<a name="metaknowledge.drop_nodesByCount"></a>metaknowledge.**drop_nodesByCount**(_grph, minCount=-inf, maxCount=inf, parameterName='count', ignoreMissing=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a graph whose nodes have a occurrence count that is within inclusive bounds of minCount and maxCount, i.e minCount <= count <= maxCount. Occurrence count is determined by reading the variable associated with the node named parameterName.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;minCount and maxCount default to negative and positive infinity respectively so without specifying either the output should be the input


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parameterName is key to count field in the node's dictionary, default is count as that is often correct

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ignoreMissing can be set False to suppress the KeyError and make nodes missing counts be dropped instead of throwing errors


- - -

<a name="metaknowledge.drop_nodesByDegree"></a>metaknowledge.**drop_nodesByDegree**(_grph, minDegree=-inf, maxDegree=inf, useWeight=True, parameterName='weight', ignoreUnweighted=True_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a graph whose nodes have a degree that is within inclusive bounds of minDegree and maxDegree, i.e minDegree <= degree <= maxDegree. Degree can be determined in two ways by default it is the total number of edges touching a node, alternative if useWeight is True it is the sum of the weight of all the edges touching a node.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;minDegree and maxDegree default to negative and positive infinity respectively so without specifying either the output should be the input

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;useWeight can be set True to use an alternative method for calculating degree, the total weight of all edges

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parameterName is key to weight field in the edge's dictionary, default is weight as that is almost always correct, only used if useWeight is True

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ignoreUnweighted can be set False to suppress the KeyError and make unweighted edges be not counted, only used if useWeight is True


- - -

<a name="metaknowledge.filterNonJournals"></a>metaknowledge.**filterNonJournals**(_citesLst, invert=False_):

# Needs to be written

- - -

<a name="metaknowledge.graphStats"></a>metaknowledge.**graphStats**(_G, stats=('nodes', 'edges', 'isolates', 'loops', 'density', 'transitivity'), makeString=True_):

# Needs to be written

- - -

<a name="metaknowledge.isiParser"></a>metaknowledge.**isiParser**(_isifile_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;isiParser() reads the file given by the path isifile, checks that the header is correct then reads until it reaches EF.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Each it finds is used to initialize a Record then all Record are returned as a list.


- - -

<a name="metaknowledge.modularity"></a>metaknowledge.**modularity**(_G, weightParameter=None, communityParameter='community'_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Gets modularity of network, currently not tuned


- - -

<a name="metaknowledge.read_graph"></a>metaknowledge.**read_graph**(_edgeList, nodeList=None, directed=False, idKey='ID', eSource='From', eDest='To'_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reads the files given by edgeList and if given nodeList and produces a networkx graph
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This is designed only for the files produced by metaknowledge and is meant to be the reverse of write_graph()

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;nodeList must be given if any of the attributes of the node are needed
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;directed controls if the resultant graph is directional eSource and eDest control the direction
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;idKey, eSource and  eDest are the labels for the edge's id, source and destination respectively, they must match headers in the file or a keyError exception will be thrown

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;igraph Style


- - -

<a name="metaknowledge.recordParser"></a>metaknowledge.**recordParser**(_paper_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reads the file _paper_ until it reaches 'ER'.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For each field tag it adds an entry to the returned dict with the tag as the key and a list of the entries as the value, the list has each line separately, so for the following string in a record:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"AF BREVIK, I

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    ANICIN, B"

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The entry in the returned dict would be `{'AF' : ["BREVIK, I", "ANICIN, B"]}`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Record](#metaknowledge.Record) objects can be created with these dictionaries as the initializer.

#####&nbsp;&nbsp;&nbsp; Parameters

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_paper_ : `file stream`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; An open file, with the current line at the beginning of the record.

#####&nbsp;&nbsp;&nbsp; Returns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dict[str : List[str]]`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A dictionary mapping WOS tags to lists, the lists are of strings, each string is a line of the record associated with the tag.


- - -

<a name="metaknowledge.write_edgeList"></a>metaknowledge.**write_edgeList**(_grph, name, extraInfo=True, progBar=None_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;writes an edge list of grph with filename name, if extraInfo is true the additional information about the edges, e.g. weight, will be written.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;All edges must have the same tags


- - -

<a name="metaknowledge.write_graph"></a>metaknowledge.**write_graph**(_grph, name, edgeInfo=True, typing=True, suffix='csv', overwrite=False_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Writes both the edge list and the node attribute list of grph.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The output files start with name, the file type[edgeList, nodeAttributes] then if typing is True the type of graph (directed or undirected) then the suffix, it appears as follows:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    name_fileType_Graphtype.suffix
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If edgeInfo is true the extra information about the edges will be included in their list, i.e. their weight will be given
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If overwrite is False write_graph will throw an exception if either of the files it is attempting to write exist


- - -

<a name="metaknowledge.write_nodeAttributeFile"></a>metaknowledge.**write_nodeAttributeFile**(_grph, name, progBar=None_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;writes a node attribute list of grph with filename name, the first column is the node's ID then all after it are its associated information.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;All nodes must have the same tags.


## <a name="metaknowledge.tagFuncs"></a>metaknowledge.**tagFuncs**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Doc String of tagFuncs


- - -

<a name="metaknowledge.tagFuncs.DOI"></a>metaknowledge.tagFuncs.**DOI**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return the DOI number of the record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DI tag


- - -

<a name="metaknowledge.tagFuncs.ISBN"></a>metaknowledge.tagFuncs.**ISBN**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of ISBNs assocaited with the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BN tag


- - -

<a name="metaknowledge.tagFuncs.ISSN"></a>metaknowledge.tagFuncs.**ISSN**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the ISSN of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SN tag


- - -

<a name="metaknowledge.tagFuncs.ResearcherIDnumber"></a>metaknowledge.tagFuncs.**ResearcherIDnumber**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a lsit of the research ids of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RI tag


- - -

<a name="metaknowledge.tagFuncs.abstract"></a>metaknowledge.tagFuncs.**abstract**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return abstract of the record, with newlines hopefully in the correct places

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AB tag


- - -

<a name="metaknowledge.tagFuncs.articleNumber"></a>metaknowledge.tagFuncs.**articleNumber**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a string giving the article number, not all are integers

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AR tag


- - -

<a name="metaknowledge.tagFuncs.authAddress"></a>metaknowledge.tagFuncs.**authAddress**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C1 tag


- - -

<a name="metaknowledge.tagFuncs.authKeyWords"></a>metaknowledge.tagFuncs.**authKeyWords**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the keywords assigned by the author of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DE tag


- - -

<a name="metaknowledge.tagFuncs.authorsFull"></a>metaknowledge.tagFuncs.**authorsFull**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of authors full names

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AF tag


- - -

<a name="metaknowledge.tagFuncs.authorsShort"></a>metaknowledge.tagFuncs.**authorsShort**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of authors shortened names

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AU tag


- - -

<a name="metaknowledge.tagFuncs.beginningPage"></a>metaknowledge.tagFuncs.**beginningPage**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the first page the record occurs on as a string not an int

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BP tag


- - -

<a name="metaknowledge.tagFuncs.bookAuthor"></a>metaknowledge.tagFuncs.**bookAuthor**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of the short names of the authors of a book Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BA tag


- - -

<a name="metaknowledge.tagFuncs.bookAuthorFull"></a>metaknowledge.tagFuncs.**bookAuthorFull**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of the long names of the authors of a book Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BF tag


- - -

<a name="metaknowledge.tagFuncs.bookDOI"></a>metaknowledge.tagFuncs.**bookDOI**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the book DOI of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;D2 tag


- - -

<a name="metaknowledge.tagFuncs.citations"></a>metaknowledge.tagFuncs.**citations**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of all the citations in the record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CR tag


- - -

<a name="metaknowledge.tagFuncs.citedRefsCount"></a>metaknowledge.tagFuncs.**citedRefsCount**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the numer citations, length of CR list

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NR tag


- - -

<a name="metaknowledge.tagFuncs.confDate"></a>metaknowledge.tagFuncs.**confDate**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the date string of the conference associated with the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CY tag


- - -

<a name="metaknowledge.tagFuncs.confHost"></a>metaknowledge.tagFuncs.**confHost**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the host of the conference

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HO tag


- - -

<a name="metaknowledge.tagFuncs.confLocation"></a>metaknowledge.tagFuncs.**confLocation**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the sting giving the confrence's location

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CL tag


- - -

<a name="metaknowledge.tagFuncs.confSponsors"></a>metaknowledge.tagFuncs.**confSponsors**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of sponsors for the conference associated with the record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SP tag


- - -

<a name="metaknowledge.tagFuncs.confTitle"></a>metaknowledge.tagFuncs.**confTitle**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the title of the conference associated with the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CT tag


- - -

<a name="metaknowledge.tagFuncs.docType"></a>metaknowledge.tagFuncs.**docType**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the type of document the Record contains

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DT tag


- - -

<a name="metaknowledge.tagFuncs.documentDeliveryNumber"></a>metaknowledge.tagFuncs.**documentDeliveryNumber**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the document delivery number of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GA tag


- - -

<a name="metaknowledge.tagFuncs.eISSN"></a>metaknowledge.tagFuncs.**eISSN**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the EISSN of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;EI tag


- - -

<a name="metaknowledge.tagFuncs.editedBy"></a>metaknowledge.tagFuncs.**editedBy**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of the editors of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BE tag


- - -

<a name="metaknowledge.tagFuncs.editors"></a>metaknowledge.tagFuncs.**editors**(_val_):

#####&nbsp;&nbsp;&nbsp; Needs Work


- - -

<a name="metaknowledge.tagFuncs.email"></a>metaknowledge.tagFuncs.**email**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of emails given by the authors of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;EM tag


- - -

<a name="metaknowledge.tagFuncs.endingPage"></a>metaknowledge.tagFuncs.**endingPage**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return the last page the record occurs on as a string not an int

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;EP tag


- - -

<a name="metaknowledge.tagFuncs.funding"></a>metaknowledge.tagFuncs.**funding**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a list of the groups funding the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FU tag


- - -

<a name="metaknowledge.tagFuncs.fundingText"></a>metaknowledge.tagFuncs.**fundingText**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns a string of the funding thanks

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FX tag


- - -

<a name="metaknowledge.tagFuncs.getMonth"></a>metaknowledge.tagFuncs.**getMonth**(_s_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Known formats:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Month ("%b")
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Month Day ("%b %d")
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Month-Month ("%b-%b") --- this gets coerced to the first %b, dropping the month range
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Season ("%s") --- this gets coerced to use the first month of the given season
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Month Day Year ("%b %d %Y")
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Month Year ("%b %Y")


- - -

<a name="metaknowledge.tagFuncs.group"></a>metaknowledge.tagFuncs.**group**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the group associated with the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GP tag


- - -

<a name="metaknowledge.tagFuncs.groupName"></a>metaknowledge.tagFuncs.**groupName**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the name of the group associated with the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CA tag


- - -

<a name="metaknowledge.tagFuncs.isoAbbreviation"></a>metaknowledge.tagFuncs.**isoAbbreviation**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the iso abbreviation of the journal

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JI tag


- - -

<a name="metaknowledge.tagFuncs.issue"></a>metaknowledge.tagFuncs.**issue**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a string giving the issue or range of issues the Record was in

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IS tag


- - -

<a name="metaknowledge.tagFuncs.j9"></a>metaknowledge.tagFuncs.**j9**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the J9 (29-Character Source Abbreviation) of the publication

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;J9 tag


- - -

<a name="metaknowledge.tagFuncs.journal"></a>metaknowledge.tagFuncs.**journal**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the full name of the publication

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SO tag


- - -

<a name="metaknowledge.tagFuncs.keyWords"></a>metaknowledge.tagFuncs.**keyWords**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the WOS keywords of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID tag


- - -

<a name="metaknowledge.tagFuncs.language"></a>metaknowledge.tagFuncs.**language**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the languages of the Record as a string with languages seperated by ', ', usually there is only one language

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;LA tag


- - -

<a name="metaknowledge.tagFuncs.makeReversed"></a>metaknowledge.tagFuncs.**makeReversed**(_d_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Simple function for reversing a dictionary


- - -

<a name="metaknowledge.tagFuncs.meetingAbstract"></a>metaknowledge.tagFuncs.**meetingAbstract**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the ID of the meeting abstract prefixed by 'EPA-'

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MA tag


- - -

<a name="metaknowledge.tagFuncs.month"></a>metaknowledge.tagFuncs.**month**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the month the record was published in as an int with January as 1, February 2, ...

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PD tag


- - -

<a name="metaknowledge.tagFuncs.orcID"></a>metaknowledge.tagFuncs.**orcID**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of orc IDs of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;OI tag


- - -

<a name="metaknowledge.tagFuncs.pageCount"></a>metaknowledge.tagFuncs.**pageCount**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns an interger giving the number of pages of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PG tag


- - -

<a name="metaknowledge.tagFuncs.partNumber"></a>metaknowledge.tagFuncs.**partNumber**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return an integer giving the part of the issue the Record is in

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PN tag


- - -

<a name="metaknowledge.tagFuncs.pubMedID"></a>metaknowledge.tagFuncs.**pubMedID**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the pubmed idof the record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PM tag


- - -

<a name="metaknowledge.tagFuncs.pubType"></a>metaknowledge.tagFuncs.**pubType**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the type of publication as a character: conference, book, journal, book in series, or patent

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PT tag


- - -

<a name="metaknowledge.tagFuncs.publisher"></a>metaknowledge.tagFuncs.**publisher**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the publisher of the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PU tag


- - -

<a name="metaknowledge.tagFuncs.publisherAddress"></a>metaknowledge.tagFuncs.**publisherAddress**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the publishers address

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PA tag


- - -

<a name="metaknowledge.tagFuncs.publisherCity"></a>metaknowledge.tagFuncs.**publisherCity**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns the city the publisher is in

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PI tag


- - -

<a name="metaknowledge.tagFuncs.reprintAddress"></a>metaknowledge.tagFuncs.**reprintAddress**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the reprint address string

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RP tag


- - -

<a name="metaknowledge.tagFuncs.seriesSubtitle"></a>metaknowledge.tagFuncs.**seriesSubtitle**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the title of the series the Record is in

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BS tag


- - -

<a name="metaknowledge.tagFuncs.seriesTitle"></a>metaknowledge.tagFuncs.**seriesTitle**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the title of the series the Record is in

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SE tag


- - -

<a name="metaknowledge.tagFuncs.specialIssue"></a>metaknowledge.tagFuncs.**specialIssue**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the special issue value

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SI tag


- - -

<a name="metaknowledge.tagFuncs.subjectCategory"></a>metaknowledge.tagFuncs.**subjectCategory**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a list of the subjects associated with the Record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SC tag


- - -

<a name="metaknowledge.tagFuncs.subjects"></a>metaknowledge.tagFuncs.**subjects**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns a lsit of subjects as assigned by WOS

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;WC tag


- - -

<a name="metaknowledge.tagFuncs.supplement"></a>metaknowledge.tagFuncs.**supplement**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the supplemtn number

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SU tag


- - -

<a name="metaknowledge.tagFuncs.title"></a>metaknowledge.tagFuncs.**title**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the title of the record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TI tag


- - -

<a name="metaknowledge.tagFuncs.totalTimesCited"></a>metaknowledge.tagFuncs.**totalTimesCited**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the total number of citations of the record

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Z9 tag


- - -

<a name="metaknowledge.tagFuncs.volume"></a>metaknowledge.tagFuncs.**volume**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return the volume the record is in as a string not an int

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;VL tag


- - -

<a name="metaknowledge.tagFuncs.wosString"></a>metaknowledge.tagFuncs.**wosString**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the WOS number of the record as a string preceded by "WOS:"

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;UT tag


- - -

<a name="metaknowledge.tagFuncs.wosTimesCited"></a>metaknowledge.tagFuncs.**wosTimesCited**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the number of times the Record has been cited byr records in WOS

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TC tag


- - -

<a name="metaknowledge.tagFuncs.year"></a>metaknowledge.tagFuncs.**year**(_val_):

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;returns the year the record was published in as an int

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PY tag

