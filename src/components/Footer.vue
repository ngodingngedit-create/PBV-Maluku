<script setup>
</script>

<template>
  <footer class="site-footer">
    <div class="footer-container">
      <div class="footer-brand">
        <div class="brand-badge">PBV MALUKU</div>
        <p class="brand-tagline">
          Bersatu • Berjuang • Berprestasi<br />
          Volleyball • Sportivitas • Kebanggaan
        </p>
      </div>

      <div class="footer-copy">
        <p>&copy; 2026 PBV Maluku Official. Powered by Kolektix.</p>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.site-footer {
  background-color: #0b0b0e;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  padding: 2.5rem 2rem;
  color: #9ca3af;
}

.footer-container {
  max-width: 1240px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1.5rem;
}

.brand-badge {
  font-weight: 800;
  font-size: 1.1rem;
  color: #ffffff;
  margin-bottom: 0.35rem;
}

.brand-tagline {
  font-size: 0.82rem;
  color: #6b7280;
  line-height: 1.5;
}

.footer-copy {
  font-size: 0.85rem;
}

@media (max-width: 640px) {
  .footer-container {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>
