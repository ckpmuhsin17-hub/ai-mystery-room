# AI Mystery Room

A responsive mystery investigation game built with React, Express, MongoDB, and Google Gemini. Players supply a fictional scenario, inspect suspects and evidence, then submit one accusation. The browser receives only public case data; the predetermined culprit and solution remain private until the accusation endpoint evaluates the guess.

## Requirements

- Node.js 20 or newer
- MongoDB (local or hosted)
- A Google Gemini API key

## Run locally

1. Install dependencies: `npm install`, `npm --prefix client install`, and `npm --prefix server install`.
2. Copy `server/.env.example` to `server/.env`.
3. Set `MONGODB_URI` and `GEMINI_API_KEY` in `server/.env`. Set `GEMINI_MODEL` to a model enabled for your key if needed.
4. Run `npm run dev` and open the Vite URL shown in the terminal. The API runs on port 3001.

The API refuses to start without MongoDB. Mystery generation returns a configuration error if no Gemini key is set; no hardcoded demo case is used as a substitute. Never expose the Gemini key through a `VITE_` variable.

## API

- `GET /api/health` — service and database status.
- `POST /api/mysteries` — generate, validate, and persist a case. Body: `scenario` (30–2,000 characters), optional `title`, `difficulty` (`Easy`, `Medium`, `Hard`), and `evidenceComplexity` (`Basic`, `Detailed`, `Extensive`).
- `GET /api/mysteries/:id` — return public investigation details only.
- `POST /api/mysteries/:id/accusations` — submit `{ "suspectId": "..." }`; evaluates once and reveals the solution.

Guest mysteries expire after seven days. API creation is rate limited. Image and CCTV evidence use clearly labeled text placeholders until a compatible media provider and cloud storage are configured.

## Build and deploy

Run `npm run build` for the production frontend bundle. Configure the Node host with MongoDB and Gemini credentials, `CLIENT_ORIGIN`, and run `npm start`. Serve `client/dist` from the same origin or configure the client and API host consistently.

