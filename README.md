<svg xmlns="http://www.w3.org/2000/svg" width="100%" height="400" viewBox="0 0 1000 400">
  <defs>
    <radialGradient id="sky" cx="50%" cy="0%" r="120%">
      <stop offset="0%" stop-color="#0b1026"/>
      <stop offset="100%" stop-color="#000010"/>
    </radialGradient>
    <style>
      .star { fill: #fff; }
      @keyframes fall {
        0%   { transform: translate(0,0) scale(1); opacity: 0; }
        10%  { opacity: 1; }
        100% { transform: translate(300px,400px) scale(0.3); opacity: 0; }
      }
      .s1 { animation: fall 4s linear infinite; }
      .s2 { animation: fall 5s linear infinite 1s; }
      .s3 { animation: fall 6s linear infinite 2s; }
      .s4 { animation: fall 3.5s linear infinite 0.5s; }
      .s5 { animation: fall 5.5s linear infinite 1.8s; }
      @keyframes twinkle { 0%,100%{opacity:.3} 50%{opacity:1} }
      .tw { animation: twinkle 2s ease-in-out infinite; }
    </style>
  </defs>

  <rect width="100%" height="100%" fill="url(#sky)"/>

  <!-- мерцающие статичные звёзды -->
  <circle class="star tw" cx="80" cy="60" r="1.4"/>
  <circle class="star tw" cx="300" cy="120" r="1.2" style="animation-delay:.4s"/>
  <circle class="star tw" cx="620" cy="40" r="1.6" style="animation-delay:.9s"/>
  <circle class="star tw" cx="880" cy="150" r="1.3" style="animation-delay:1.3s"/>
  <circle class="star tw" cx="450" cy="200" r="1.1" style="animation-delay:.7s"/>
  <circle class="star tw" cx="150" cy="260" r="1.5" style="animation-delay:1.6s"/>

  <!-- падающие звёзды -->
  <circle class="star s1" cx="100" cy="20" r="2"/>
  <circle class="star s2" cx="350" cy="0"  r="1.8"/>
  <circle class="star s3" cx="600" cy="-20" r="2.2"/>
  <circle class="star s4" cx="200" cy="-40" r="1.6"/>
  <circle class="star s5" cx="750" cy="-10" r="1.9"/>
</svg>
