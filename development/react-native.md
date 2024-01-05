# React Native

## 🎓 J'ai compris et je peux expliquer

- les différences et points communs entre du code react et du code react native ✔️
- ce que devient et comment est interprêté le code javascript dans une application react native ✔️
- les avantages et inconvénients de react native ✔️
- l'utilisation de react native avec expo ✔️
- les principales briques qui composent react native (core components) ❌
- comment écrire du style en react native ✔️
- comment est géré le layout en react native ❌

## 💻 J'utilise

### Un exemple personnel commenté ✔️

```javascript
// Login: A functional component for handling user login.
// - setUserContext: A function for setting the user context after successful login.
function Login({ setUserContext }) {
  // State for managing login details (email and password).
  const [loginDetails, setLoginDetails] = useState({
    email: "",
    password: "",
  });

  // loginUser: Function to handle the login process when the form is submitted.
  const loginUser = (e) => {
    e.preventDefault(); // Prevents the default form submit action.
    axios
      .post(`${PORT_BACKEND}/login`, loginDetails) // Makes a POST request to the backend login endpoint.
      .then((res) => {
        setUserContext(res.data); // Sets the user context with the response data.
        AsyncStorage.setItem(
          "Eco44Token",
          JSON.stringify({
            userToken: res.data.userToken, // User authentication token.
            userId: res.data.userId, // User's unique identifier.
          })
        );
      })
      .catch((err) => console.error("error", err)); // Logs any error that occurs during login.
  };

  // Render: Returns the JSX for the login form.
  return (
    <View style={styles.login}>
      <TextInput
        keyboardType="email-address"
        style={styles.input}
        placeholder="Email"
        name="email"
        onChangeText={
          (value) => setLoginDetails({ ...loginDetails, email: value }) // Updates state with the entered email.
        }
        required
      />
      <TextInput
        style={styles.input}
        placeholder="Password"
        name="password"
        onChangeText={
          (value) => setLoginDetails({ ...loginDetails, password: value }) // Updates state with the entered password.
        }
        secureTextEntry
        required
      />
      <Button title="LOGIN" onPress={loginUser} /> // Button to trigger the
      login process.
    </View>
  );
}
// StyleSheet: Defines the styles for the login component.
const styles = StyleSheet.create({
  login: {
    flex: 1,
    padding: 20,
    marginTop: 10,
  },
  input: {
    height: 40,
    borderColor: "gray",
    borderWidth: 1,
    marginBottom: 10,
    padding: 10,
  },
});

export default Login;
```

### Utilisation dans un projet ✔️

[lien github](https://github.com/Megakrash/Eco44App)

Description : Version mobile en cours de développent d'un site vitrine ficif actuellement en production

## 🌐 J'utilise des ressources

### Expo

- (https://docs.expo.dev/)
