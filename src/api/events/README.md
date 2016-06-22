* Can't use defineProperty. Will not trigger on properties added to model. 
Can only use Proxy or setter

/**
 * Mutate function on domainObject
 */

domainObject.mutate("property", function (currentVal) {
    return "12";
});

//No way of mutating multiple properties at once.
// Removes need to calculate deltas to trigger
/*domainObject.mutate(function (domainObject) {
    object.foo = "bar";
    return object;
});*/

//Mutating an array property
domainObject.mutate("arrayProperty", function (array) {
    array.push("foo");
    array[2] = "bar";
    //mutate function can calculate delta to decide
    // whether to trigger update
    return object;
});

domainObject.mutate("property.nestedProperty", function(nestedValue){
    return "someNewNestedValue"
});

/*
Problem here - no way to prevent arbitrary mutation
 of the object from within the mutate function.
 */
domainObject.mutate("property.nestedProperty", function(nestedObject){
    nestedObject.someprop = "someVal";
    nestedObject.something = {};
    nestedObject.something.else = "Hi";
    return nestedObject;
});

/**
 * Hybrid setter / mutator approach
 */
domainObject.mutate("property", "value");
domainObject.mutate("property.nestedProperty", "value");
domainObject.mutate("arrayProperty", function(array){
    array.push("foo");
    array[1] = "bar";
});
// OR
// Mutate the array outside and use the same approach
domainObject.mutate("arrayProperty", arrayVal);
/**
 * Setter function on domainObject. I think this is
 * out because of the difficulty in support Arrays
 */
//Setters are out because of arrays I think
domainObject.set("property", value);
domainObject.set("property.nestedProperty", value);

domainObject.on("configuration.layout");
domainObject.on("configuration.layout.positions");

// The following would trigger configuration.views
// watcher, but not configuration.views.view1
domainObject.set("configuration.layout", {"positions": [{"123-1234-1232-123": [100, 200]}]});

// Don't think this will work, it's an unnatural way
// of using js.

//Using a setter for everything requires unnatural
// use of javascript
domainObject.set("configuration", {})
    .set("layout", {})
    .set("positions", [{"123-1234-1232-123": [100, 200]}]);

// VanillaJS
domainObject.model.configuration = {};
domainObject.model.configuration.layout = {};
domainObject.model.configuration.positions = [{"123-1234-1232-123": [100, 200]}];

// Layout does exist
domainObject.set("configuration")
    .set("layout")
    .set("positions", [{"123-1234-1232-123": [100, 200]}]);