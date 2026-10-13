# ButchAI

ButchAI was a subscription web app I built and deployed on my own in 2024. You upload a short video, and the app watches it, writes a narration, and gives it back to you as spoken audio.

## What it does

- **Video narration:** pulls key frames from an uploaded video with OpenCV, has GPT-4o-mini describe each frame, combines the descriptions into one script, and turns it into speech with ElevenLabs.
- **Text-to-speech:** a separate tool that converts typed text to audio.
- **Accounts:** sign-up with email verification codes, login, forgot username/password, and password changes. Passwords are stored hashed.
- **Subscriptions:** Basic and Premium plans with recurring billing through Stripe Checkout, plus a webhook that keeps each user's plan status in sync.
- **Usage tracking:** logs AI token usage, video minutes, and cost per user so each plan stays profitable.
- **Security:** CSRF protection, rate limiting (Flask-Limiter), hCaptcha on forms, and input sanitizing with bleach.

## Tech stack

Python · Flask · SQLite · Redis · OpenCV · OpenAI API · ElevenLabs API · Stripe · Gunicorn · Nginx · DigitalOcean (Droplet + Spaces)

## Project layout

| File | Purpose |
| --- | --- |
| `app.py` | Flask routes: auth, billing, uploads, TTS |
| `audio_utils.py` / `basic_audio_utils.py` | Frame extraction, AI descriptions, and audio generation for each plan |
| `stripe_utils.py` | Stripe checkout and subscription helpers |
| `db_utils.py` | SQLite connection and user table |
| `redis_config.py` | Redis connection for rate limiting |
| `send_email.py` | Verification and reset emails over SMTP |
| `tts_token_logging.py` | Per-user token and cost logging |
| `templates/`, `static/` | HTML pages, CSS, and JavaScript |

## Running it locally

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # then fill in your own keys
python app.py
```

You'll need your own OpenAI, ElevenLabs, Stripe, and hCaptcha keys, a Redis instance, and an SMTP account. In production it ran under Gunicorn behind Nginx on a DigitalOcean droplet.

## Status

The live site has been taken down. This repo is shared as a portfolio project.
