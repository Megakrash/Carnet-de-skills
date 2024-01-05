# Les bases de données et l'environnement node.js

## 🎓 J'ai compris et je peux expliquer

- Comment développer en utilisant un système de livereloading (`nodemon`) ✔️
- Comment développer en utilisant une base de données SQL (`MySQL` / `SQLite` / `PostgreSQL`) ✔️
- La connexion de mon application à une base de données avec et sans ORM/ODM (`MySQL` avec `mysql2` ou `knex` et `PostgreSQL` avec `TypeORM`) ✔️
- Le développement d'une API REST et GraphQL (avec les packages `express` et `graphql`) ✔️
- La création d'un context et la configuration des `CORS`✔️
- La manipulation des fichiers (avec `fs` et `multer`) ✔️
- Le hash des données sensibles avant enregistrement en DB (avec `argon2`) ✔️
- Comment mettre en place un système d'authentification et de connexion-déconnexion complet. Création de tokens à stocker dans le `Local Storage` ou de cookies dans le navigateur (avec `JWT` et `Cookies`) ✔️
- Sécurisation des Resolvers avec `@Authorized()` ✔️
- La gestion des emails (avec `nodeMailer`) ✔️

## 💻 J'utilise

### Deux exemples personnels commentés ✔️

#### Utilisation de `fs` et `knex` avec MySQL

```javascript
// This function removes the picture associated with a brand from the database and file system.
// - req (Request): The HTTP request object containing the brand ID and picture information.
// - res (Response): The HTTP response object used to send back the status.
const deleteBrandPicByBrandId = (req, res) => {
  // Parse the brand ID from the request parameters.
  const id = parseInt(req.params.id, 10);

  // Extract the picture filename from the request body.
  const { pic } = req.body;

  // Start a new database transaction.
  knex
    .transaction((trx) => {
      // Update the brand record in the database by setting the picture field to null.
      return trx("brands")
        .where("id", id)
        .update("pic", null)
        .then(() => {
          // Check if the picture file exists in the file system.
          if (fs.existsSync(`public/assets/images/brands/${pic}`)) {
            // Delete the picture file from the file system.
            fs.unlink(`public/assets/images/brands/${pic}`, (err) => {
              // Log an error if the file deletion fails.
              if (err) {
                console.error(err);
              }
            });
            // Send a 204 No Content response to indicate successful deletion.
            res.sendStatus(204);
          } else {
            // Warn if the file doesn't exist in the file system.
            console.warn("file doesn't exist!");
          }
        });
    })
    .catch((err) => {
      // Log any errors that occur during the transaction.
      console.error(err);
    });
};
```

#### Utilisation de `argon2`, `JWT`, `graphql`, `TypeORM` avec `PostgreSQL`

```javascript
// userLogin: Authenticates a user by their email and password, and issues a JWT token if successful.
// - context (MyContext): The context object containing HTTP request and response objects.
// - data (UserLoginInput): An object containing the user's email and password.
// @Mutation(() => User): Indicates that this is a GraphQL mutation that returns a User object.
@Mutation(() => User)
async userLogin(
  @Ctx() context: MyContext, // Injects the context into the function.
  @Arg("data", () => UserLoginInput) data: UserLoginInput // Retrieves and validates the user login data from the GraphQL arguments.
) {
  // Search for a user in the database with the provided email.
  const user = await User.findOne({ where: { email: data.email } });
  // If the user does not exist, throw an error indicating wrong email or password.
  if (!user) {
    throw new Error("Wrong email or password");
  }

  // Verify the password using argon2, a secure password hashing function.
  const valid = await argon2.verify(user.hashedPassword, data.password);
  // If the password is incorrect, throw an error.
  if (!valid) {
    throw new Error("Wrong email or password");
  }

  // Generate a JWT token with the user's ID and an expiry time of 1 hour.
  const token = jwt.sign(
    {
      exp: Math.floor(Date.now() / 1000) + 60 * 60, // Sets the expiration time of the token.
      userId: user.id, // Embeds the user's ID in the token.
    },
    process.env.JWT_SECRET_KEY || "" // Uses the secret key from the environment variables.
  );

  // Create a cookie with the JWT token and set it in the response.
  const cookie = new Cookies(context.req, context.res); // Initializes a new cookie instance.
  cookie.set("TGCookie", token, { // Sets the cookie name as "TGCookie" with the generated token.
    httpOnly: true, // Makes the cookie inaccessible to client-side scripts for security.
    secure: false, // Sets the cookie to be used over HTTP (false) or HTTPS (true).
    expires: new Date(Date.now() + 2 * 60 * 60 * 1000), // Sets the cookie to expire in 2 hours.
  });
  // Return the authenticated user.
  return user;
}
```

### Utilisation dans un projet ✔️

[lien github](https://github.com/Megakrash/the-good-corner/tree/main)

Description : Projet en cours de développement dans le cadre de la formation pour la préparation du titre "Application developer designer" au sein de la Wild Code School.

### Utilisation en production ✔️

[lien Ecophone 44](https://ecophone44.megakrash.com/)

Description : Site vitrine fictif en production ou la plus grande partie du travail est dans une partie réservée à l'administrateur.

## 🌐 J'utilise des ressources

### Apollo graphQl

- (https://www.apollographql.com/docs/)

### Knex

- (https://knexjs.org/guide/)
