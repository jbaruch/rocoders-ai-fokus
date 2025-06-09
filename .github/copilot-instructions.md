# Copilot Instructions: Vibecoding RGBW Control App

These instructions are for an agentic coding assistant (e.g., Copilot Agent in VS Code) to implement a real-time RGBW smart bulb control demo app for the Vibecoding conference.

> ⚠️ This project intentionally favors simplicity for a live coding session. Avoid production-level complexity like cloud dependencies, persistent storage, or build chains.

---

## 🧠 Agent Environment

This project assumes:

* **VS Code with Copilot Agent Mode** is active.
* **Context7 MCP is fully configured and running.**

  * Use it extensively for all Java 21, Spring Boot 3.5, jmdns/mDNS discovery APIs.
  * Use it for DOM APIs, `getUserMedia`, `enumerateDevices`, and Puppeteer scripting.

---

## 🔧 Backend Responsibilities (Spring Boot 3.5, Java 21)

* Implement REST endpoint `POST /color` to receive `{ red, green, blue }` JSON.
* Implement `GET /discover` to perform mDNS-based discovery of a Shelly Duo RGBW bulb.
* Use `RestClient` to invoke `http://<bulb-ip>/light/0` with query parameters: `turn=on`, RGBW values, `gain=100`.
* White = `min(R,G,B)`
* Log and return errors for:

  * Device not found
  * Timeout or bad HTTP status

### 🧭 Discovery Quirk

* mDNS discovery must **not** bind to the loopback interface (127.0.0.1).
* Ensure you're using the correct non-loopback, non-virtual, IPv4 interface.
* Context7 is required for checking jmdns usage and filtering network interfaces.

### ✅ Backend Testing

* Use JUnit 5 and Mockito for all controller and discovery logic.
* Tests must validate:

  * RGB → RGBW conversion
  * Correct URL formation for Shelly
  * Error handling paths

---

## 🌐 Frontend Responsibilities (HTML + JS)

* Use plain HTML/JS with optional `htm/preact` or `lit-html` (from CDN).
* Use `getUserMedia()` for webcam access.
* Use `enumerateDevices()` to show a **camera selector dropdown**.
* Show a live preview of the detected color.
* Button to manually send RGBW color to backend.
* Auto mode sends color every 3 seconds.
* Fetch `/discover` on load to resolve bulb IP.

### ✅ Frontend Testing

* Use Puppeteer MCP to:

  * Load the app
  * Interact with toggle/button/selector
  * Take screenshots
  * Verify DOM state and visual feedback

---

## 📋 Reference

* These instructions are derived from the canonical [Requirements](../docs/Requirements.md) document.
* Use it for detailed functional scope, exclusions, and demo constraints.

## ✅ General Rules

* Use Context7 for **every code block** involving Spring Boot, jmdns, frontend browser APIs, or Puppeteer.
* Do not use WebSocket, database, Redis, Docker, MQTT, or cloud services.
* Make each step demonstrable and readable for a live conference audience.
* Favor clarity over optimization.
* Commit incrementally with meaningful messages.
