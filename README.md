# userApiJs
kantinak a feltoltott izeje: import express from "express";
import users from "./data/users.js";

const PORT = 3000;
const app = express();

app.use(express.json());

app.get("/users", (req, res) => {
	res.json(users);
});

app.get("/users/:id", (req, res) => {
	const id = req.params.id;
	if (id < 0 || id >= users.length) {
		return res.json({ message: "User not found" });
	}
	res.json(users[id]);
});

app.post("/users", (req, res) => {
	const { firstName, lastName } = req.body;
	if (!firstName || !lastName) {
		return res.json({ message: "Missing name" });
	}
	users.push({ firstName, lastName });
	res.json(users[users.length - 1]);
});

app.put("/users/:id", (req, res) => {
	const id = req.params.id;
	if (id < 0 || id >= users.length) {
		return res.json({ message: "User not found" });
	}
	const { firstName, lastName } = req.body;
	if (!firstName || !lastName) {
		return res.json({ message: "Missing name" });
	}
	users[id] = { firstName, lastName };
	res.json(users[id]);
});

app.patch("/users/:id", (req, res) => {
	const id = req.params.id;
	if (id < 0 || id >= users.length) {
		return res.json({ message: "User not found" });
	}
	const { firstName, lastName } = req.body;
	users[id] = {
		firstName: firstName || users[id].firstName,
		lastName: lastName || users[id].lastName,
	};
	res.json(users[id]);
});

app.delete("/users/:id", (req, res) => {
	const id = req.params.id;
	if (id < 0 || id >= users.length) {
		return res.json({ message: "User not found" });
	}
	users.splice(id, 1);
	res.json({ message: "Delete successful" });
});

app.listen(PORT, () => {
	console.log(`Server runs on port http://localhost:${PORT}/`);
});
