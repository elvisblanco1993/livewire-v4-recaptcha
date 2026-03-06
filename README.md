# Livewire ReCAPTCHA v3 / v2 / v2 Invisible

A lightweight package that adds **Google reCAPTCHA protection to Livewire components** using a simple directive and attribute.

This package supports:

* Google **reCAPTCHA v3**
* Google **reCAPTCHA v2**
* Google **reCAPTCHA v2 Invisible**
* **Laravel**
* **Livewire v3 and Livewire v4**

Developed and maintained by **Code Wize Technologies LLC**.

---

# Installation

```
composer require elvisblanco1993/livewire-v4-recaptcha
```

---

# Configuration

Read Google's documentation to create your reCAPTCHA keys:

[https://developers.google.com/recaptcha/intro](https://developers.google.com/recaptcha/intro)

Each version requires its **own site key and secret key pair**.

---

## Supported Versions

| Version              | Documentation                                                                                                    | Notes                       |
| -------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------- |
| **v3 (recommended)** | [https://developers.google.com/recaptcha/docs/v3](https://developers.google.com/recaptcha/docs/v3)               | Score-based verification    |
| **v2**               | [https://developers.google.com/recaptcha/docs/display](https://developers.google.com/recaptcha/docs/display)     | Standard checkbox           |
| **v2 Invisible**     | [https://developers.google.com/recaptcha/docs/invisible](https://developers.google.com/recaptcha/docs/invisible) | Use `'size' => 'invisible'` |

---

# Laravel Configuration

Add the configuration to **config/services.php**

## reCAPTCHA v3

```
'google' => [
    'recaptcha' => [
        'site_key' => env('GOOGLE_RECAPTCHA_SITE_KEY'),
        'secret_key' => env('GOOGLE_RECAPTCHA_SECRET_KEY'),
        'version' => 'v3',
        'score' => 0.5,
    ],
],
```

`score` must be between **0 and 1** and determines the minimum trust threshold.

---

## reCAPTCHA v2

```
'google' => [
    'recaptcha' => [
        'site_key' => env('GOOGLE_RECAPTCHA_SITE_KEY'),
        'secret_key' => env('GOOGLE_RECAPTCHA_SECRET_KEY'),
        'version' => 'v2',
        'size' => 'normal',
        'theme' => 'light',
    ],
],
```

Available options:

```
size: normal | compact | invisible
theme: light | dark
```

---

# Usage

## Livewire Component

Add the `#[ValidatesRecaptcha]` attribute to the method that processes the form.

```
use Livewire\Component;
use ElvisBlanco1993\LivewireRecaptcha\ValidatesRecaptcha;

class ContactForm extends Component
{
    public string $gRecaptchaResponse;

    #[ValidatesRecaptcha]
    public function submit()
    {
        // This only runs if the captcha passes
    }
}
```

If the `$gRecaptchaResponse` property is not declared, it will be **automatically created**.

---

## Advanced Configuration

You can override the secret key or score directly:

```
#[ValidatesRecaptcha(secretKey: 'mysecretkey', score: 0.9)]
```

This is useful if you want **different captcha strictness per form**.

---

# Blade Usage

## Add the Livewire Directive

Attach the directive to the form you want protected.

```
<form wire:submit="submit" wire:recaptcha>
    <!-- form fields -->

    <button type="submit">
        Submit
    </button>
</form>
```

---

## Add the Script Directive

Add the Blade directive once on the page:

```
@livewireRecaptcha
```

You can add it:

* in the **layout**
* or **only on pages with forms**

---

## Error Handling

When verification fails, a validation error is thrown for:

```
gRecaptchaResponse
```

Example:

```
@if ($errors->has('gRecaptchaResponse'))
    <div class="text-red-600">
        {{ $errors->first('gRecaptchaResponse') }}
    </div>
@endif
```

The translation key is:

```
livewire-recaptcha::recaptcha.invalid_response
```

---

# Overriding Settings in Blade

You can override any configuration directly:

```
@livewireRecaptcha(
    version: 'v2',
    siteKey: 'your_site_key',
    theme: 'dark',
    size: 'compact'
)
```

---

# How It Works

1. The form is submitted.
2. The `wire:recaptcha` directive intercepts the request.
3. A **reCAPTCHA token is generated client-side**.
4. The token is **validated server-side against Google**.
5. If validation passes, the Livewire method runs.

If validation fails, the request is rejected.

---

# Compatibility

| Package  | Supported |
| -------- | --------- |
| Laravel  | 10+       |
| Livewire | v3        |
| Livewire | v4        |

---

# Security

Always validate reCAPTCHA **server-side**, which this package does automatically.

Never trust client responses alone.

---

# License

MIT License
