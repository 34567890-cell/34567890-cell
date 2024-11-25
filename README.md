- 👋 Hi, I’m @34567890-cell
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
34567890-cell/34567890-cell is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
// Get the canvas and context
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

// Set canvas size
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

// Game variables
let isJumping = false;
let jumpHeight = 0;
let score = 0;
let gameSpeed = 3;
let obstacles = [];
let lastObstacleTime = 0;

// Character
const character = {
    x: 50,
    y: canvas.height - 70,
    width: 50,
    height: 50,
    color: 'orange',
    draw: function() {
        ctx.fillStyle = this.color;
        ctx.fillRect(this.x, this.y - jumpHeight, this.width, this.height);
    }
};

// Obstacle
function createObstacle() {
    const obstacle = {
        x: canvas.width,
        y: canvas.height - 50,
        width: 30 + Math.random() * 50,
        height: 50,
        draw: function() {
            ctx.fillStyle = 'red';
            ctx.fillRect(this.x, this.y, this.width, this.height);
        }
    };
    obstacles.push(obstacle);
}

// Update the game state
function updateGame() {
    // Clear the canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Character jump logic
    if (isJumping) {
        if (jumpHeight < 150) {
            jumpHeight += 5;
        } else {
            isJumping = false;
        }
    } else if (jumpHeight > 0) {
        jumpHeight -= 5;
    }

    // Draw character
    character.draw();

    // Handle obstacles
    if (Date.now() - lastObstacleTime > 1500) {
        createObstacle();
        lastObstacleTime = Date.now();
    }
    obstacles.forEach((obstacle, index) => {
        obstacle.x -= gameSpeed;
        obstacle.draw();

        // Check for collision
        if (
            character.x < obstacle.x + obstacle.width &&
            character.x + character.width > obstacle.x &&
            character.y - jumpHeight < obstacle.y + obstacle.height
        ) {
            alert('Game Over! Your score: ' + score);
            resetGame();
        }

        // Remove off-screen obstacles
        if (obstacle.x + obstacle.width < 0) {
            obstacles.splice(index, 1);
            score++;
        }
    });

    // Draw score
    ctx.fillStyle = 'white';
    ctx.font = '20px Arial';
    ctx.fillText('Score: ' + score, 20, 30);

    requestAnimationFrame(updateGame);
}

// Handle key events
document.addEventListener('keydown', (e) => {
    if (e.key === ' ' && !isJumping) {
        isJumping = true;
    }
});

// Reset the game
function resetGame() {
    obstacles = [];
    score = 0;
    isJumping = false;
    jumpHeight = 0;
}

// Start the game loop
updateGame();
