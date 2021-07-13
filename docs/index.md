---
permalink: /
layout: home
title: Overview
lead: >
  Use the login.gov Design System when developing login.gov sites to consistently identify the login.gov brand.
---

<section class="usa-section usa-section--dark">
  <div class="grid-container">
    <div class="grid-row">
      <div class="maxw-tablet">
        <div class="usa-display">
          The key to a strong brand <br>is <span class="text-accent-cool">consistency</span>.
        </div>
        <p class="usa-intro">{{ page.lead }}</p>
        <a href="https://github.com/18F/identity-style-guide#configuring-for-development" class="usa-button usa-button--big">See install instructions on GitHub</a>
      </div>
    </div>
  </div>
</section>

<section class="usa-section">
  <div class="grid-container">
    <div class="grid-row grid-gap-4">
      <div class="tablet:grid-col">
<div class="usa-prose margin-bottom-2 tablet:margin-bottom-0" markdown="1">

# Installs in seconds

Install images, stylesheets, and script files in both their original and compiled forms with npm.

</div>
      </div>
      <div class="tablet:grid-col">
<div class="usa-prose margin-bottom-2 tablet:margin-bottom-0" markdown="1">

# Built with USWDS

The {{ site.title }} is built using the <a href="https://v2.designsystem.digital.gov/" target="_blank">U.S. Web Design System</a> for a consistent appearance across government sites.

</div>
      </div>
      <div class="tablet:grid-col">
<div class="usa-prose margin-bottom-2 tablet:margin-bottom-0" markdown="1">

# A work in progress

The latest version is <strong class="text-no-wrap">{{ site.package_json.this_version }}</strong> which uses <strong class="text-no-wrap">uswds@{{ site.package_json.uswds_version }}</strong>. Spot an issue? We’d love to hear about it <a href="https://github.com/18F/identity-style-guide/issues" target="_blank">on GitHub</a>.

</div>
      </div>
    </div>
  </div>
</section>
