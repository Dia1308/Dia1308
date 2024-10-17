{
  "name": "your health",  // Remplace par le nom de ton projet
  "version": "1.0.0",     // Version de ton projet
  "description": "Une application incroyable.",  // Brève description
  "main": "index.js",     // Fichier principal
  "scripts": {
    "start": "node index.js",  // Commande pour démarrer le projet
    "test": "jest"              // Commande pour les tests (si tu utilises Jest)
  },
  "dependencies": {
    "express": "^4.17.1",      // Exemple de dépendance pour le back-end
    "react": "^17.0.1"          // Exemple de dépendance pour le front-end (si tu utilises React)
  },
  "devDependencies": {
    "nodemon": "^2.0.4"         // Dépendance de développement pour le rechargement automatique
  },
  "author": "Ton Nom",        // Remplace par ton nom
  "license": "ISC"            // Type de licence
}npx react-native init SportsApp
cd SportsApp
npm install axiosimport React, { useState } from 'react';
import { View, TextInput, Button, Text, StyleSheet } from 'react-native';
import axios from 'axios';

const SignUp = () => {
  const [age, setAge] = useState('');
  const [weight, setWeight] = useState('');
  const [workoutPlan, setWorkoutPlan] = useState(null);

  const handleSignUp = async () => {
    try {
      const response = await axios.post('http://localhost:3000/signup', {
        age,
        weight
      });
      setWorkoutPlan(response.data.workoutPlan);
    } catch (error) {
      console.error('Error signing up:', error);
    }
  };

  return (
    <View style={styles.container}>
      <Text>Inscription</Text>
      <TextInput
        placeholder="Âge"
        keyboardType="numeric"
        value={age}
        onChangeText={setAge}
        style={styles.input}
      />
      <TextInput
        placeholder="Poids"
        keyboardType="numeric"
        value={weight}
        onChangeText={setWeight}
        style={styles.input}
      />
      <Button title="S'inscrire" onPress={handleSignUp} />
      {workoutPlan && (
        <View style={styles.plan}>
          <Text>Plan sportif :</Text>
          <Text>{workoutPlan.day1}</Text>
          <Text>{workoutPlan.day2}</Text>
        </View>
      )}
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    padding: 20,
    marginTop: 50
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 10,
    padding: 10
  },
  plan: {
    marginTop: 20
  }
});

export default SignUp;mkdir sports-app-backend
cd sports-app-backend
npm init -y
npm install express mongoose corsconst express = require('express');
const cors = require('cors');
const mongoose = require('mongoose');

const app = express();
app.use(cors());
app.use(express.json());

mongoose.connect('mongodb://localhost:27017/sportsApp', { useNewUrlParser: true, useUnifiedTopology: true });

// Schéma utilisateur
const userSchema = new mongoose.Schema({
  age: Number,
  weight: Number
});

const User = mongoose.model('User', userSchema);

// Route d'inscription
app.post('/signup', async (req, res) => {
  const { age, weight } = req.body;
  
  // Logique simple pour définir un plan sportif en fonction de l'âge et du poids
  let workoutPlan = {};
  
  if (age > 20 && age < 30 && weight >= 70 && weight <= 80) {
    workoutPlan = {
      day1: '15 Push-ups, 20 Squats',
      day2: '30 min de course, 1 min de planche'
    };
  } else {
    workoutPlan = {
      day1: '10 Push-ups, 15 Squats',
      day2: '20 min de marche, 30 secondes de planche'
    };
  }

  // Sauvegarde des informations dans la base de données
  const user = new User({ age, weight });
  await user.save();

  res.json({ message: 'Inscription réussie', workoutPlan });
});

app.listen(3000, () => {
  console.log('Serveur en marche sur le port 3000');
});mongod
