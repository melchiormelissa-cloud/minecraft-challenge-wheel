const challenges = [
  'Find Diamonds',
  'Survive 10 Days',
  'No Armor Run',
  'Build a Village',
  'Only One Block',
  'Beat the Ender Dragon',
  'Make a Nether Base',
  'Mine 100 Iron',
  'Ride a Horse',
  'Craft a Full Set',
  'Go Fishing',
  'No Bed Challenge',
  'Steal a Villager',
  'Mine 10 Emeralds',
  'Slay 20 Zombies',
  'Build a Bridge',
  'Defeat a Warden',
  'Grow a Sugar Cane Farm',
  'Collect 3 Biomes',
  'Create a Secret Base'
];

const colors = [
  '#2d7db9', '#5eb95d', '#d9b036', '#b75bd6', '#d66d6d',
  '#3ca38c', '#d58b38', '#7a7ae5', '#de6f9d', '#7bbd9a',
  '#e2b52f', '#7ca2d9', '#b96ad1', '#7ec5a2', '#d06b55',
  '#67b8d9', '#d28548', '#7abf46', '#d7d462', '#9d65d8'
];

const wheel = document.getElementById('wheel');
const spinBtn = document.getElementById('spinBtn');
const result = document.getElementById('result');

let currentRotation = 0;

function buildWheel() {
  const segmentAngle = 360 / challenges.length;
  const gradientStops = challenges.map((challenge, index) => {
    const start = index * segmentAngle;
    const end = (index + 1) * segmentAngle;
    return `${colors[index % colors.length]} ${start}deg ${end}deg`;
  }).join(', ');

  wheel.style.background = `conic-gradient(${gradientStops})`;

  const radius = 160;
  const center = 220;

  challenges.forEach((challenge, index) => {
    const label = document.createElement('div');
    label.className = 'wheel-label';

    const angle = index * segmentAngle + segmentAngle / 2;
    const radians = (angle - 90) * (Math.PI / 180);
    const x = center + Math.cos(radians) * radius;
    const y = center + Math.sin(radians) * radius;

    label.style.left = `${x}px`;
    label.style.top = `${y}px`;
    label.style.transform = `translate(-50%, -50%) rotate(${angle}deg)`;

    const text = document.createElement('span');
    text.textContent = challenge;
    label.appendChild(text);
    wheel.appendChild(label);
  });
}

function spinWheel() {
  if (spinBtn.disabled) return;

  spinBtn.disabled = true;
  result.textContent = 'Spinning...';

  const segmentAngle = 360 / challenges.length;
  const extraTurns = 7 * 360 + Math.random() * 360;
  currentRotation += extraTurns;
  wheel.style.transform = `rotate(${currentRotation}deg)`;

  const normalizedDegrees = (360 - (currentRotation % 360) + 360) % 360;
  const winnerIndex = Math.floor(normalizedDegrees / segmentAngle) % challenges.length;

  setTimeout(() => {
    result.textContent = challenges[winnerIndex];
    spinBtn.disabled = false;
  }, 4200);
}

spinBtn.addEventListener('click', spinWheel);
buildWheel();
