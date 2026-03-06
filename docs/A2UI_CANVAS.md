# A2UI Canvas Hook - Experimental

The Canvas allows the agent to push UI components directly to the user's screen (macOS/iOS/Android).

```javascript
// Example Canvas Push
openclaw.canvas.push({
  title: "Ecosystem Status",
  type: "grid",
  data: ["Nanobot", "NanoClaw", "PicoClaw", "ZeroClaw"]
});
```

- **Features**: Live-eval, Screen recording, Gesture input.
