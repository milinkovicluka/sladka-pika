# Kontakt

Za naročilo torte ali dodatna vprašanja nas pokličite, pišite na WhatsApp ali Viber, ali pa izpolnite obrazec spodaj. Odgovorimo v enem dnevu.

<div class="contact-buttons">
  <a class="btn" href="tel:{{ site.phone }}">Pokliči {{ site.phone_display }}</a>
  <a class="btn btn-whatsapp" href="https://wa.me/{{ site.whatsapp }}?text=Pozdravljeni%2C%20zanima%20me%20torta." rel="noopener">WhatsApp</a>
  <a class="btn btn-viber" href="viber://chat?number=%2B{{ site.whatsapp }}">Viber</a>
  <a class="btn btn-ghost" href="mailto:{{ site.email }}">{{ site.email }}</a>
</div>

Torte naročite vsaj teden dni pred dogodkom, poročne vsaj mesec prej. Prevzem: {{ site.location }}.

## Obrazec za naročilo

<form id="narocilo" class="order-form" action="#narocilo" method="POST" data-demo>
  <label>Ime in priimek *<input type="text" name="ime" autocomplete="name" required></label>
  <label>Telefon *<input type="tel" name="telefon" autocomplete="tel" inputmode="tel" required></label>
  <label>E-pošta<input type="email" name="email" autocomplete="email"></label>
  <label>Vrsta torte *<select name="vrsta" required><option value="">Izberite ...</option><option>Rojstnodnevna</option><option>Otroška</option><option>Poročna</option><option>Čokoladna</option><option>Jagodna</option><option>Cupcakes</option><option>Drugo / po meri</option></select></label>
  <div class="form-row">
    <label>Datum prevzema *<input type="date" name="datum" id="datum" required></label>
    <label>Število oseb<input type="number" name="osebe" min="1" max="300" inputmode="numeric"></label>
  </div>
  <label>Opombe (okus, napis, alergije, ideja ...)<textarea name="opombe" rows="4"></textarea></label>
  <button class="btn" type="submit">Pošlji povpraševanje</button>
  <p class="form-note">Povpraševanje ni zavezujoče. Naročilo potrdimo skupaj po telefonu ali e-pošti.</p>
  <p class="form-demo-msg" id="demo-msg" hidden>To je demo: obrazec ne pošilja. Na pravi strani bi povpraševanje prispelo na e-pošto podjetja.</p>
</form>
<script>
  (function () {
    var d = new Date();
    d.setDate(d.getDate() + 7);
    var el = document.getElementById("datum");
    if (el) el.min = d.toISOString().slice(0, 10);
    var f = document.getElementById("narocilo");
    if (f) f.addEventListener("submit", function (e) {
      e.preventDefault();
      document.getElementById("demo-msg").hidden = false;
    });
  })();
</script>

[< Nazaj domov](/)
