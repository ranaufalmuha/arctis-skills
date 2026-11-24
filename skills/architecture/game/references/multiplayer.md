# Networked gameplay and dedicated servers

Use only when the task involves multiplayer. A dedicated server running engine simulation is a game application. Account, commerce, or leaderboard HTTP services remain separate service applications; apply their architecture independently.

- Identify the authoritative owner of each replicated state and the permitted origin of commands. Validate remote intent at the authority boundary; client presentation does not grant authority.
- Keep gameplay rules in the simulation owner. Transport adapters encode/decode and dispatch accepted intent; they do not become the only place rules exist.
- Separate predicted client state from authoritative state and specify reconciliation if prediction is used. Do not assume every game needs rollback, lockstep, or prediction.
- Version wire contracts deliberately. Share stable schemas or protocol definitions across applications without importing client UI/assets into a server.
- Treat spawn/despawn, reconnect, scene transitions, and ownership transfer as lifecycle changes. Define behavior for duplicated, late, or out-of-order messages as required by the transport.
- A headless target should not require presentation initialization. Verify the actual server build and bootstrap instead of merely hiding the window.

Test authority rejection and one relevant connection lifecycle in addition to the happy path. Use the project's chosen networking stack and engine-version documentation; do not introduce another network framework to satisfy this skill.
