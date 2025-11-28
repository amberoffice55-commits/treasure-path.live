---
name: Custom issue template
about: Describe this issue template's purpose here.
title: ''
labels: ''
assignees: ''

---

// app.js - interactions and small animations

// set year
document.getElementById('year').textContent = new Date().getFullYear();

// Animated counters (digit-by-digit accurate)
function animateCounter(id, target, prefix = '', duration = 1500) {
  const el = document.getElementById(id);
  if (!el) return;
  const start = 0;
  const startTime = performance.now();
  function tick(now) {
    const t = Math.min(1, (now - startTime) / duration);
    const ease = t < 0.5 ? 2*t*t : -1 + (4 - 2*t)*t; // smooth ease
    const value = Math.floor(start + (target - start) * ease);
    el.textContent = prefix + value.toLocaleString();
    if (t < 1) requestAnimationFrame(tick);
  }
  requestAnimationFrame(tick);
}

// Demo numbers
animateCounter('counter-days', 230);
animateCounter('counter-users', 4821);

(function(){
  const deposits = 142954;
  const withdraw = 51902;
  const elDep = document.getElementById('counter-deposits');
  const elW = document.getElementById('counter-withdraw');

  function moneyAnim(el, target, duration){
    const startTime = performance.now();
    const start=0;
    function tick(now){
      const t = Math.min(1, (now - startTime)/duration);
      const value = Math.floor(start + (target-start)*(t));
      el.textContent = '$' + value.toLocaleString();
      if (t < 1) requestAnimationFrame(tick);
    }
    requestAnimationFrame(tick);
  }
  moneyAnim(elDep, deposits, 1400);
  moneyAnim(elW, withdraw, 1400);
})();

// FAQ toggle
document.querySelectorAll('.faq .item').forEach(item=>{
  item.addEventListener('click', ()=>{
    item.classList.toggle('open');
  });
  item.setAttribute('tabindex','0');
  item.addEventListener('keydown', (e)=> {
    if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); item.click(); }
  });
});

// Contact form (demo - no backend). Validate fields, show status.
function validEmail(e){
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(e);
}

function sendContact(){
  const name = document.getElementById('name').value.trim();
  const email = document.getElementById('email').value.trim();
  const message = document.getElementById('message').value.trim();
  const status = document.getElementById('contact-status');

  if (!name || !email || !message){
    status.style.color = getComputedStyle(document.documentElement).getPropertyValue('--danger') || '#ff6b6b';
    status.textContent = 'Please complete all fields.';
    return;
  }
  if(!validEmail(email)){
    status.style.color = getComputedStyle(document.documentElement).getPropertyValue('--danger') || '#ff6b6b';
    status.textContent = 'Please provide a valid email address.';
    return;
  }

  status.style.color = getComputedStyle(document.documentElement).getPropertyValue('--success') || '#18c985';
  status.textContent = 'Message queued — demo only (no real email sent).';

  // Clear form after a short pause (simulate send)
  setTimeout(()=> {
    document.getElementById('name').value='';
    document.getElementById('email').value='';
    document.getElementById('message').value='';
  },900);
}

function resetContact(){
  document.getElementById('contact-status').textContent = '';
  document.getElementById('name').value='';
  document.getElementById('email').value='';
  document.getElementById('message').value='';
}

// button wiring
document.getElementById('send-message').addEventListener('click', sendContact);
document.getElementById('reset-message').addEventListener('click', resetContact);

// simple button actions
document.getElementById('btn-get-started').addEventListener('click', ()=>{
  window.location.hash = '#signup';
  alert('Signup flow — demo only. Replace with your sign-up page or modal.');
});
document.getElementById('btn-learn-more').addEventListener('click', ()=>{
  window.location.hash = '#packages';
});

// invest buttons
document.getElementById('invest-starter').addEventListener('click', ()=>{
  window.location.hash = '#signup';
  alert('Invest (Starter) — demo only.');
});
document.getElementById('invest-advanced').addEventListener('click', ()=>{
  window.location.hash = '#signup';
  alert('Invest (Advanced) — demo only.');
});
document.getElementById('contact-sales').addEventListener('click', (e)=>{
  e.preventDefault();
  document.querySelector('#contact').scrollIntoView({behavior:'smooth'});
});

// small progressive reveal
window.addEventListener('load', ()=> {
  document.querySelectorAll('.hero-card, .card-stats, .plan').forEach((el,i)=>{
    el.style.opacity = 0;
    el.style.transform = 'translateY(8px)';
    setTimeout(()=> {
      el.style.transition = 'opacity .5s ease, transform .5s ease';
      el.style.opacity = 1;
      el.style.transform = 'translateY(0)';
    }, 120 + i*80);
  });
});
