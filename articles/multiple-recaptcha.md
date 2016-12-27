title: Run Multiple Google reCAPTCHA on the same page
photo: v1479110874/google-recaptcha_hwfpx5.jpg
tags: [security, javascript, front-end, google]
intro: How to run multiple Google reCAPTCHA on the same page.
---

This article covers how to install, configure and enable multiple instances of
Google reCAPTCHA on the same page. It doesn't cover how to handle the reCAPTCHA
validation and submission to Google.

## Install Google reCAPTCHA

First thing first, install the `Google reCAPTCHA`. Add the following code to
the bottom of your page, inside the `body` tag.

```
<script src="https://www.google.com/recaptcha/api.js?onload=onloadCallback&render=explicit" async defer></script>
```

## Build your form

Add the following field to your form to display the `Google reCAPTCHA`.
Notice the `class` attribute. It will be used later by our javascript code to
display the multiple instances. The `id` attribute is optional and may help to
customize the field if needed.

```
<div class="form-group">
    <div class="g-recaptcha" id="optional-id"></div>
</div>

```

## Custom Javascript

Add the following code to the bottom of your page, inside the `body` tag.
Replace `your-google-reCAPTCHA-site-key-value` by your `Google Site Key`.

```
<script type="text/javascript" charset="utf-8">
    var onloadCallback = function() {
        var recaptchas = document.querySelectorAll('div[class=g-recaptcha]');

        for( i = 0; i < recaptchas.length; i++) {
            grecaptcha.render( recaptchas[i].id, {
            'sitekey' : 'your-google-reCAPTCHA-site-key-value',
            });
        }
    }
</script>
```

Happy coding.
