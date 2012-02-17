# JSON hashing
json-hash is a way to hash json-compatible datam, indendent of it's actuall representation. It aims towards being language and hash-algorithm agnostic and might work with other data structures than the ones created by json (although we make a few assumptions that will only be valid for json)

## Current support

* 

## Usage

### Javascript

    var data = {'list': ['element1', {'key': 'value'}, 'element2'], 'foo': 'bar'};
    var hashingFunction = function(string) {
        return hex_sha256(string); // You can use this one: http://pajhome.org.uk/crypt/md5/
    };
    var hash = jsonHash(
        data, 
        hashingFunction,
        {
            sortMaps: true,            // Sort Maps before hashing, defaults to false 
            sortArrays: true,          // Sort Arrays before hashing, defaults to false
            normalizeKeyCasing: false  // Convert all keys to lower case before hashing 
        }
    );
    console.log(hash);