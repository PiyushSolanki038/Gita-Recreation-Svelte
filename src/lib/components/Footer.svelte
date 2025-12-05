<footer class="site-footer">
  <div class="footer-inner">
    
    <!-- CONNECT SECTION -->
    <div class="footer-connect">
      <span class="footer-connect-label">Connect</span>

      <div class="footer-icons">
        <a href="#" class="icon-circle"><img src="/facebook.png" alt="fb" /></a>
        <a href="#" class="icon-circle"><img src="/twitter.png" alt="twitter" /></a>
        <a href="#" class="icon-circle"><img src="/youtube.png" alt="youtube" /></a>
        <a href="#" class="icon-circle"><img src="/compass.png" alt="compass" /></a>
      </div>
    </div>

    <!-- CENTER LOGO -->
    <div class="footer-logo">
      <img src="/android-chrome-192x192.png" alt="WELLBEING-SVASTI" />
    </div>

    <!-- BRAND NAME -->
    <h2 class="footer-brand">WELLBEING~SVASTI</h2>

    <!-- COPYRIGHT -->
    <p class="footer-copy">
      © 2025 Rutger Kortenhorst. All Rights Reserved | Design and Developed by
    </p>

    <!-- CREDIT -->
    <a href="#" class="footer-credit">Burning Desire Inclusive</a>
  </div>
</footer>

<style>
.site-footer {
    background-image: url('/footerimg.png'); 
  background-color: #2f3b2f;
  color: #f7e9c4;
  padding: 36px 20px 40px;
  text-align: center;
  background-size: cover;
}

.footer-inner {
  max-width: 900px;
  margin: 0 auto;
}

/* ========= CONNECT SECTION ========= */

.footer-connect {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 18px;
  margin-bottom: 20px;
}

.footer-connect-label {
  font-family: "Cinzel", "Times New Roman", serif;
  font-size: 28px;
  letter-spacing: 1px;
  font-weight: 400;
  color: #f6ead1;
}

.footer-icons {
  display: flex;
  gap: 12px;
}

.icon-circle {
  width: 46px;
  height: 46px;
  border-radius: 50%;
  border: 2px solid rgba(246, 234, 209, 0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  transition: 0.2s ease;
}

.icon-circle img {
  width: 22px;
  height: 22px;
  display: block;
}

.icon-circle:hover {
  background: #235839;
  border-color: #f6ead1;
  transform: scale(1.05);
}

/* ========= LOGO ========= */

.footer-logo img {
  height: 90px;
  margin: 4px 0 18px 0;
}

/* ========= BRAND ========= */

.footer-brand {
  font-family: "Cinzel", serif;
  font-size: 50px;
  font-weight: 400;
  margin-top: -10px;
  margin-bottom: 10px;
  color: #f7e9c4;
}

/* ========= COPYRIGHT ========= */

.footer-copy {
  margin: 0 0 4px 0;
  font-family: "Inter", system-ui, sans-serif;
  font-size: 15px;
  color: #f7e9c4;
}

/* ========= CREDIT ========= */

.footer-credit {
  font-family: "Inter", system-ui, sans-serif;
  font-size: 16px;
  color: #41b2ae;
  text-decoration: none;
  margin-top: 6px;
  display: inline-block;
}

.footer-credit:hover {
  text-decoration: underline;
}
/* ==============================
   FOOTER – RESPONSIVE DESIGN
============================== */

@media (max-width: 768px) {
  .footer-logo img {
    height: 70px;
  }

  .footer-brand {
    font-size: 22px;
  }

  .footer-copy {
    font-size: 14px;
  }

  .icon-circle {
    width: 40px;
    height: 40px;
  }

  .footer-connect-label {
    font-size: 22px;
  }
}

@media (max-width: 480px) {
  .footer-brand {
    font-size: 18px;
  }

  .footer-logo img {
    height: 60px;
  }

  .footer-copy {
    font-size: 12px;
  }

  .icon-circle {
    width: 34px;
    height: 34px;
  }
}

</style>
