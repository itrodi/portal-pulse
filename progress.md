Original prompt: okay would need you to build the portal pulse as a single page html , css and javascript game and you can make use phaser if needed 1. Add this script tag to load the SDK in the head section:
<script src="https://cdn.jsdelivr.net/npm/@remix-gg/sdk@latest/dist/index.min.js"></script>

2. Add these SDK calls to the game:

// When the game is over (replace scoreValue with the actual score):
window.RemixSDK.singlePlayer.actions.gameOver({ score: scoreValue });

// For haptic feedback on important interactions (like jumping or hitting obstacles):
window.RemixSDK.hapticFeedback();

// Add this to handle play again requests:
window.RemixSDK.onPlayAgain(() => {
  // Reset the game state here
  // For example: resetGame(), startNewGame(), etc.
});

// REQUIRED: Add this to handle mute/unmute - MUST be included even if the game doesn't currently have sound.
// This is non-negotiable and required for integration checks to pass.
// If the game has no audio, you can leave the callback empty or add a placeholder:
window.RemixSDK.onToggleMute((data) => {
  // Set game audio based on data.isMuted
  // For example: setMuted(data.isMuted)
  // If game has no sound, you can leave this empty but the handler MUST exist
});

3. Call gameOver when the player loses or completes the game.
4. Add haptic feedback for important game events.
5. IMPORTANT: The toggle_mute handler (window.FarcadeSDK.onToggleMute) MUST be included in your code, even if the game has no sound. This is required for integration checks and cannot be skipped.  the game has to have a 2:3 aspect ratio and integrate with the sdk above the game has to be suitable for single players and have as many levels as possible

Notes:
- Need to address feedback: longer levels (~1 min), better obstacle density, moving camera, correct player sprite.
- Must add render_game_to_text + advanceTime hooks and run Playwright loop per skill.

Updates:
- Rewrote game loop with procedural long levels (360+ tiles each, 10 levels), more obstacles, checkpoints, springs, hearts, stars, water, trees, clouds, and portal lock (80% coins).
- Added camera follow + shake, health system, invulnerability, dash/coin gating, and title overlay.
- Added render_game_to_text + advanceTime hooks and fullscreen toggle.
- Updated HUD to use digit tiles and icons for score/coins/hearts.

Testing:
- Ran Playwright client with actions file (2 iterations) against http://localhost:5173/index.html.
- Reviewed output/web-game/shot-0.png and shot-1.png; visuals and HUD visible, obstacles present.
- Ran an additional Playwright pass with custom inputs to verify camera movement; shot-0.png shows mid-level view and Game Over overlay.
