// Firebase configuration
var firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);

// Initialize Cloud Firestore
var db = firebase.firestore();

// Fonction pour récupérer les commerces d'événementiel sans site web dans une région spécifique
function trouverCommercesEvenementielSansSite(region) {
  var placesService = new google.maps.places.PlacesService(document.createElement('div'));
  var request = {
    query: 'commerces événementiels',
    region: region
  };

  placesService.textSearch(request, function(results, status) {
    if (status === google.maps.places.PlacesServiceStatus.OK) {
      results.forEach(function(place) {
        // Vérifier si le commerce n'a pas de site web
        if (!place.website) {
          // Enregistrer le commerce dans Firestore
          db.collection("commerces").add({
            name: place.name,
            address: place.formatted_address
          });
        }
      });
    }
  });
}

// Exemple d'utilisation
var region = "Paris, France"; // Remplacez par la région de votre choix
trouverCommercesEvenementielSansSite(region);
