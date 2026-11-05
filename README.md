# Reporter Hasan TTS — Retrain (Nov 2)

বাংলা লেখা লিখলে Nov 2 retrain মডেল — ১,৫৯৫টি ক্লিপের ট্রেনিং (97ep "KIKI HASAN Big"-এর ঠিক আগের মডেল) কণ্ঠে অডিও বানায় — ব্রাউজারেই শোনা যায়, WAV ডাউনলোড করা যায়।

## কী আছে

- `app.py` — Streamlit UI (বাংলা): লেখা → অডিও + প্লেয়ার + WAV ডাউনলোড, দুটো নমুনা লেখা
- `tts_engine.py` — মডেল ডাউনলোড (size + MD5 যাচাই), দাঁড়ি-ভিত্তিক chunking (≤৭০০ অক্ষর/ভাগ), sherpa-onnx synthesis
- `requirements.txt` — `sherpa-onnx==2.0.1`, `streamlit`, `numpy` (torch নেই, ভারী কিছু নেই)

## মডেল কোথা থেকে আসে

মডেল ফাইল (১১৪ MB) এই repo-তে **নেই** — GitHub-এর ১০০ MB সীমার বেশি। অ্যাপ প্রথমবার চালু হলে public Hugging Face repo `demoqqq/banglatts-eleven-new-retrain` থেকে `model.onnx` (MD5 `05d0cd86…`) আর `tokens_sherpa.txt` (MD5 `bec67f31…`) নামিয়ে size + MD5 মিলিয়ে নেয়, তারপর cache করে।

মডেলের পরিচয়: ২ নভেম্বর ২০২৬-এর retrain — ১,৪২২টি প্রথম ব্যাচ + ১৭৩টি elefin ক্লিপ, EMTIAZZ base থেকে fresh fine-tune, ৮৫-টোকেন vocab। 97ep মডেলের আগের সংস্করণ; তুলনার জন্য রাখা হয়েছে।

## Streamlit Community Cloud-এ deploy

1. এই repo GitHub-এ push করুন (public)।
2. [share.streamlit.io](https://share.streamlit.io)-তে GitHub দিয়ে সাইন-ইন করুন → **New app** → repo, branch `main`, main file `app.py` বেছে **Deploy** চাপুন।
3. লাইভ লিংক পাবেন `https://<app-name>.streamlit.app` আকারে।

## সত্যি সীমাগুলো

- ফ্রি tier **CPU-মাত্র, ~১ GB RAM**, GPU নেই — ১ মিনিটের অডিও বানাতে ~১.৫–২.৫ মিনিট লাগতে পারে।
- ১২ ঘণ্টা কেউ না এলে অ্যাপ ঘুমিয়ে যায়; প্রথম যে খুলবে তার ~১ মিনিট অপেক্ষা করতে হবে (মডেল লোড + wake)।
- এটা শুধু অনলাইন ডেমো — অফলাইন Android অ্যাপই আসল প্রোডাক্ট।

## স্থানীয়ভাবে চালানো

```bash
python3 -m venv venv
venv/bin/pip install -r requirements.txt
venv/bin/streamlit run app.py
```
