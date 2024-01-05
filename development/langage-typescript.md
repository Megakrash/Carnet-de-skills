# TypeScript

## 🎓 J'ai compris et je peux expliquer

- l'intéret de TypeScript dans l'IDE ✔️
- les types de bases ✔️
- comment et pourquoi étendre une interface ✔️
- les classes et les decorators ✔️

## 💻 J'utilise

### Un exemple personnel commenté ✔️

```javascript
// UserConnection: A functional component for user authentication.
const UserConnection = (): React.ReactNode => {
  // State hooks for managing email and password.
  const [email, setEmail] = useState < string > "";
  const [password, setPassword] = useState < string > "";

  // handlePasswordChange: Function to update the password state.
  const handlePasswordChange = (newPassword: React.SetStateAction<string>) => {
    setPassword(newPassword);
  };

  // useMutation: GraphQL mutation hook for user login.
  const [doLogin] = useMutation(mutationUserLogin, {
    refetchQueries: [queryMeContext],
  });

  // onSubmit: Function to handle form submission.
  const onSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault(); // Prevents the default form submit action.
    try {
      // Executes the login mutation with email and password variables.
      const { data } = await doLogin({
        variables: { data: { email, password } },
      });

      // Check for successful login and display a welcome message.
      if ("id" in data.item) {
        toast(`Connexion réussie, bienvenue ${data.item.nickName}`, {
          style: { background: "#0fcc45", color: "#fff" },
        });

        // Redirects to the account page after a delay.
        setTimeout(() => {
          router.replace(`/compte`);
        }, 1500);
      }
    } catch (error) {
      // Displays an error message if login fails and resets email and password.
      toast("Email ou mot de passe incorrect", {
        style: { background: "#e14d2a", color: "#fff" },
      });
      setEmail("");
      setPassword("");
    }
  };

  // JSX for rendering the component.
};
```

### Utilisation dans un projet ✔️

[lien github](https://github.com/Megakrash/the-good-corner/tree/main)

Description : Projet en cours de développement dans le cadre de la formation pour la préparation du titre "Application developer designer" au sein de la Wild Code School.

## 🌐 J'utilise des ressources

### TypeScript doc

- https://www.typescriptlang.org/docs/

### TypeORM doc

- https://typeorm.io/
