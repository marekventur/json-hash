# JSON hashing
json-hash is a way to hash json-compatible datam, indendent of it's actuall representation. It aims towards being language and hash-algorithm agnostic and might work with other data structures than the ones created by json (although we make a few assumptions that will only be valid for json)

Goals:

* Implementation independant
* Laguage independent
* Parser independent
* Representation independant
* No collison between logically different input values

## Current support

* Nothing (atm)

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
            sortObject: true,          // Sort Maps before hashing, defaults to true 
            sortArrays: false,         // Sort Arrays before hashing, defaults to false
            normalizeKeyCasing: false  // Convert all keys to lower case before hashing, defaults to false
        }
    );
    console.log(hash);

## Rules
Those rules are used to hash a data object

### hash(String): String
1. Replace every ":" in input with "::"
2. Add leading "string:"
3. Use provided hashing function to hash input and return result

### hash(Number): String
1. Convert integer in String. Don't use anything but 0-9 "," and (if necessary) a leading "-"
2. Add leading "number:"
3. Use provided hashing function to hash generated string and return result

### hash(Null): String
1. Return hash("null")

### hash(boolean): String
1. Return hash(input?"true":"false")

### hash(Object): String
<ol>
    <li>stringBuffer = "hash"</li>
    <li>[If option "sortObject" is true] Sort object by key (See "Sorting")</li>
    <li>
        For every key/value in input:
        <ol>
            <li>[If option "normalizeKeyCasing" is true] convert key to lowercase</li>
            <li>stringBuffer = stringBuffer + ":" + hash(key)</li>
            <li>stringBuffer = stringBuffer + ":" + hash(value)</li>
        </ol>
    </li>
    <li>Return hash(stringBuffer)</li>
</ol>

### hash(Array): String
<ol>
    <li>stringBuffer = "array"</li>
    <li>[If option "sortArray" is true] Sort array (See "Sorting")</li>
    <li>
        For every element in input
        <ol>
            <li>stringBuffer = stringBuffer + ":" + hash(elemtent)</li>
        </ol>
    </li>
    <li>Return hash(stringBuffer)</li>
</ol>

### Sort:
Sort by character, ascending. 
That means:
    "a" < "b" < "c"
    "aa" < "ab"