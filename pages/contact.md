---
layout: page
title: "Kontakt"
meta_title: "Contact Us"
subheadline: ""
teaser: ""
permalink: "/contact/"
---

<div class="py2">
    <form action="https://formspree.io/rafal@paliwoda.eu" method="POST" class="form-stacked">
      <input type="text" name="email" class="field-light" placeholder="{{ site.contact.email }}">
      <textarea type="text" name="content" class="field-light" rows="5" placeholder="{{ site.contact.content }}"></textarea>
      <input type="hidden" name="_next" value="{{ site.baseurl }}/thanks/" />
      <input type="hidden" name="_subject" value="{{ site.contact.subject }}" />
      <input type="text" name="_gotcha" style="display:none" />
      <input type="submit" class="button button-blue button-big mobile-block" value="{{ site.contact.submit }}">
    </form>
</div>