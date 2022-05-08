# refactored-doodle

Workshop on REST API best practices

## Technology/tools used:
- REST API & Documentation
- Three-tiered Architecture
- Postman
- SwaggerUI

## To use:
Run `yarn install` and `yarn run dev` on your machine after cloning.

### Main project structure

|____refactored-doodle
|    |____public
|         |____index.html
|         |____css
|              |____styles.css
|         |____js
|              |____app.js
|    |____src
|         |____controllers
|              |____app.js
|         |____database
|              |____db.json
|              |____Workout.js
|         |____services
|              |____workoutService.js
|         |____v1
|              |____routes
|                   |____workoutRoutes.js
|         |____index.js

1. Setup and config express/scripts

```javascript

// In src/index.js

const express = require("express");
const app = express();

const PORT = process.env.PORT || 3000;

// For testing purposes

app.get("/", (req, res) => {
	res.send("<h2>It's Working!</h2>");
});

app.listen(PORT, () => {
	console.log(`API is listening on port ${PORT}`);
});

```

  

In the package.json file:

```javascript

"scripts": {
	"dev": "nodemon src/index.js"
}

```

2. Version the API
	Within the structure of the API, ensure you have all of the initial functionality in a `v1` subfolder (the subsequent `v2`, and so on).
	- Create an `index.js` file
	- Create a simple router similar to the one previously mentioned in the previous step
	- Hook up router for v1 inside root entry point inside `src/index.js`:
	```javascript
	/ In src/index.js
	const express = require("express");
	const v1Router = require("./v1/routes");
	
	const app = express();
	const PORT = process.env.PORT || 3000;
	
	app.use("/api/v1", v1Router);
	
	app.listen(PORT, () => { 
		console.log(`API is listening on port ${PORT}`);
	});
	```
3. Name resources in plural
	Where you may have multiple controllers/services/routes/etc., it's best for clarity's sake to keep the folders named in plural.

4. Define endpoints and test them: 

```javascript
// In src/v1/routes/workoutRoutes.js
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => {
  res.send("Get all workouts");
});

router.get("/:workoutId", (req, res) => {
  res.send("Get an existing workout");
});

router.post("/", (req, res) => {
  res.send("Create a new workout");
});

router.patch("/:workoutId", (req, res) => {
  res.send("Update an existing workout");
});

router.delete("/:workoutId", (req, res) => {
  res.send("Delete an existing workout");
});

module.exports = router;
```
5. Hook v1 router into entry point (`src/index.js`):
```javascript
    // In src/index.js
const express = require("express");
const v1WorkoutRouter = require("./v1/routes/workoutRoutes");

const app = express();
const PORT = process.env.PORT || 3000;

app.use("/api/v1/workouts", v1WorkoutRouter);

app.listen(PORT, () => {
  console.log(`API is listening on port ${PORT}`);
});
```

6. Create a controller method for each endpoint and refactor router:

````javascript
/ In src/controllers/workoutController.js
const getAllWorkouts = (req, res) => {
  res.send("Get all workouts");
};

const getOneWorkout = (req, res) => {
  res.send("Get an existing workout");
};

const createNewWorkout = (req, res) => {
  res.send("Create a new workout");
};

const updateOneWorkout = (req, res) => {
  res.send("Update an existing workout");
};

const deleteOneWorkout = (req, res) => {
  res.send("Delete an existing workout");
};

module.exports = {
  getAllWorkouts,
  getOneWorkout,
  createNewWorkout,
  updateOneWorkout,
  deleteOneWorkout,
};

//in v1/routes/workoutRoutes.js

router.get("/", workoutController.getAllWorkouts);

router.get("/:workoutId", workoutController.getOneWorkout);

router.post("/", workoutController.createNewWorkout);

router.patch("/:workoutId", workoutController.updateOneWorkout);

router.delete("/:workoutId", workoutController.deleteOneWorkout);
``````

7. Create service layer:
   ```javascript
    const getAllWorkouts = () => {
  return;
};

const getOneWorkout = () => {
  return;
};

const createNewWorkout = () => {
  return;
};

const updateOneWorkout = () => {
  return;
};

const deleteOneWorkout = () => {
  return;
};

module.exports = {
  getAllWorkouts,
  getOneWorkout,
  createNewWorkout,
  updateOneWorkout,
  deleteOneWorkout,
};
   ```
   "It's also a good practice to name the service methods the same as the controller methods so that you have a connection between those. Let's start off with just returning nothing."
8. Use methods inside of workout controller so it can communicate with our service layer:

```javascript
    const workoutService = require("../services/workoutService");

const getAllWorkouts = (req, res) => {
  const allWorkouts = workoutService.getAllWorkouts();
  res.send("Get all workouts");
};

const getOneWorkout = (req, res) => {
  const workout = workoutService.getOneWorkout();
  res.send("Get an existing workout");
};

const createNewWorkout = (req, res) => {
  const createdWorkout = workoutService.createNewWorkout();
  res.send("Create a new workout");
};

const updateOneWorkout = (req, res) => {
  const updatedWorkout = workoutService.updateOneWorkout();
  res.send("Update an existing workout");
};

const deleteOneWorkout = (req, res) => {
  const updatedWorkout = workoutService.updateOneWorkout();
  res.send("Delete an existing workout");
};

module.exports = {
  getAllWorkouts,
  getOneWorkout,
  createNewWorkout,
  updateOneWorkout,
  deleteOneWorkout,
};
```

9. Create DB/DB files. In this project, `db.json` is created for the data and `Workout.js` for the workout-specific methods.
10. Create a data-access layer and return all workouts from the `db.json` file.
11. To be able to parse the request body sent back, install the `body-parser` package and configure it in the `src/index.js` file.

[OG source project - via FreeCodeCamp](https://www.freecodecamp.org/news/rest-api-design-best-practices-build-a-rest-api/)