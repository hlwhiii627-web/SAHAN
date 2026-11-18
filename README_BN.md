# E7T AUDIO — Android APK Starter

## এই ZIP-এ কী আছে
- E7T AUDIO-এর responsive dark UI
- ৪–৬ সংখ্যার basic PIN screen lock
- IndexedDB-তে audio file সংরক্ষণ
- Search, play, previous/next, delete
- GitHub Actions workflow দিয়ে cloud-এ Android debug APK build

## গুরুত্বপূর্ণ নিরাপত্তা সীমাবদ্ধতা
এটি একটি **starter build**, সম্পূর্ণ secure vault নয়।
- PIN hash করা হলেও এটি encryption নয়; একই অ্যাপের web storage/ডাটাবেস থেকে ফাইল সুরক্ষিত থাকে না।
- অডিও IndexedDB-তে সাধারণ Blob হিসেবে থাকে।
- Fingerprint/Android BiometricPrompt এখনো যুক্ত করা হয়নি।
- Screen off/background playback-এর জন্য Android Media3 foreground service এখনো যুক্ত করা হয়নি।
- Encrypted backup/restore এখনো যুক্ত করা হয়নি।
- অ্যাপের data clear বা uninstall করলে স্থানীয় অডিও হারাতে পারে। মূল ফাইলের আলাদা কপি রাখুন।

এই ফিচারগুলো যোগ না করে অ্যাপটিকে “encrypted vault” হিসেবে ব্যবহার করবেন না।

## শুধু Android ফোন দিয়ে APK বানানোর পদ্ধতি
1. GitHub account খুলুন বা সাইন ইন করুন।
2. GitHub-এ নতুন repository তৈরি করুন, নাম দিন `E7T-AUDIO`.
3. এই ZIP extract করে সব ফাইল repository-তে upload করুন। `.github/workflows/android-apk.yml` ফাইলটির path ঠিক একই থাকতে হবে।
4. Repository-র **Actions** tab খুলুন।
5. “Build E7T AUDIO APK” workflow নির্বাচন করুন।
6. **Run workflow** চাপুন।
7. Build শেষ হলে workflow run-এর নিচে **Artifacts** থেকে `E7T-AUDIO-debug-APK` ZIP ডাউনলোড করুন।
8. ZIP extract করে `app-debug.apk` ফাইল Android-এ install করুন। প্রথমবার Android Settings-এ browser/file manager-কে “Install unknown apps” অনুমতি দিতে হতে পারে। শুধু নিজের তৈরি APK-ই install করুন।

## Build ব্যর্থ হলে
Actions → failed run → Build log খুলে error message দেখুন। পুরো error text কপি করে সাহায্য চাইতে পারেন।

## পরের development ধাপ
Production-ready সংস্করণে যোগ করতে হবে:
1. Native Android BiometricPrompt
2. Android Media3 ExoPlayer + MediaSession foreground service
3. Android Keystore-এ সুরক্ষিত key management ও AES-GCM file encryption
4. Passphrase-protected encrypted export/import backup
5. PIN retry delay, app lifecycle lock এবং data migration

Biometric authentication-কে encryption key হিসেবে সরাসরি ব্যবহার করা যাবে না; Android Keystore-এ রাখা key ব্যবহারের অনুমতি দেওয়ার জন্য biometric gate লাগাতে হবে।
