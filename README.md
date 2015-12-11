Dropin replacement for the downloaded / compiled react-helmet package because i could not get master working with 
Internet Explorer

````javascript
var updateTags = function updateTags(type, tags) {
  var headElement = document.head || document.querySelector("head");
  var existingTags = headElement.querySelectorAll(type + "[" + HELMET_ATTRIBUTE + "]");
  var newTagsArray = [];
  var existingTagsArray = [];
  Array.forEach(existingTags, function(tag) { existingTagsArray.push(tag); });

  if (tags && tags.length) {
    tags
      .reverse()
      .forEach(function(tag)  {
        var newElement = document.createElement(type);

        for (var attribute in tag) {
          if (tag.hasOwnProperty(attribute)) {
            newElement.setAttribute(attribute, tag[attribute]);
          }
        }
        newElement.setAttribute(HELMET_ATTRIBUTE, "true");

        var exists = false;
        Array.some(existingTagsArray, function(existingTag, key) {
          if (exists === false && newElement.isEqualNode(existingTagsArray[key])) {
            existingTagsArray.splice(key, 1);
            exists = true;
          }
        });

        if (exists === false) {
          newTagsArray.push(newElement);
        }
      });
    Array.forEach(existingTagsArray, function(tag) { tag.parentNode.removeChild(tag); });
    Array.forEach(newTagsArray, function(tag) { headElement.insertBefore(tag, headElement.firstChild); });
  }
};
````
