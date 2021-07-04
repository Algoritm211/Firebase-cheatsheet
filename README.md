# Set a document
When you use set() to create a document, you must specify an ID for the document to create.
```javascript
// Add a new document in collection "cities"
db.collection("cities").doc("LA").set({
    name: "Los Angeles",
    state: "CA",
    country: "USA"
})
db.collection('cities').doc('BJ').set({
    capital: true
}, { merge: true });
```

Specify that the data should be **merged into the existing document**.
```javascript
db.collection('cities').doc('BJ').set({
    capital: true
}, { merge: true });
```

# Add a document
Let Cloud Firestore auto-generate an ID.

```javascript
// Add a new document with a generated id.
db.collection("cities").add({
    name: "Tokyo",
    country: "Japan"
});
```

```.add(...)``` and ```.doc().set(...)``` are completely equivalent
```javascript
// Add a new document with a generated id.
db.collection("cities").doc().set(data);
```

# Update a document
```javascript
// Set the "capital" field of the city 'DC'
db.collection("cities").doc("DC").update({
    capital: true
});
```

Update fields in nested objects
```javascript
// Create an initial document to update.
let frankDocRef = db.collection("users").doc("frank");
frankDocRef.set({
    name: "Frank",
    favorites: { food: "Pizza", color: "Blue", subject: "recess" },
    age: 12
});
// To update age and favorite color:
db.collection("users").doc("frank").update({
    "age": 13,
    "favorites.color": "Red"
});
```
Add server timestamps to your documents, when updating the field.
```javascript
// Update the timestamp field with the value from the server
db.collection('objects').doc('some-id').update({
    timestamp: firebase.firestore.FieldValue.serverTimestamp()
});
```
Update elements in an array
```javascript
let washingtonRef = db.collection("cities").doc("DC");
// Atomically add a new region to the "regions" array field.
washingtonRef.update({
    regions: firebase.firestore.FieldValue.arrayUnion("greater_virginia")
});
// Atomically remove a region from the "regions" array field.
washingtonRef.update({
    regions: firebase.firestore.FieldValue.arrayRemove("east_coast")
});
```

# Get a document
```javascript
db.collection("cities").doc("SF")
  .get()
  .then(function(doc) {
    if (doc.exists) {
      console.log("Document data:", doc.data());
    } else {
      // doc.data() will be undefined in this case
      console.log("No such document!");
    }
  }).catch(function(error) {
    console.log("Error getting document:", error);
  });
```

Get multiple documents from a collection
```javascript
db.collection("cities").get()
  .then(function(querySnapshot) {
    querySnapshot.forEach(function(doc) {
      // doc.data() is never undefined for query doc snapshots
      console.log(doc.id, " => ", doc.data());
  });
});
```

# Delete document
Delete document by id
```javascript
let docRef = Firebase.firestore().collection("Rooms").doc("bsYNIwEkjP237Ela6fUp").collection("Messages");

docRef.doc("lKjNIwEkjP537Ela6fhJ").delete();
```


# Perform simple and compound queries

Simple queries
```javascript
db.collection("cities").where("capital", "==", true);
db.collection("cities").where("state", "==", "CA");
db.collection("cities").where("population", "<", 100000);
db.collection("cities").where("name", ">=", "San Francisco");
db.collection("cities").where("regions", "array-contains", "west_coast");
```

Compound queries
```javascript
db.collection("cities").where("state", ">=", "CA").where("state", "<=", "IN");
db.collection("cities").where("state", "==", "CA").where("population", ">", 1000000);
```
