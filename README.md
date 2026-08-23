# समरस मध्यप्रदेश — V2.4

यह V2 package पुराने presigned-URL implementation को हटाकर Cloudflare R2 Workers Multipart Upload पर आधारित है।

## Files
- `upload.html` — अधिकृत कार्यकर्ता का मोबाइल upload page
- `admin.html` — authorized uploader registry
- `worker.js` — Google token verification + area policy + R2 multipart API
- `wrangler.toml` — R2/KV bindings
- `README.md` — setup guide

## Area policies
100 / 200 / 300 / 400 / 500 MB per file.
साथ में daily quota और daily file limit।

## Google setup
1. Google Cloud Console में Web OAuth/Google Identity Services client बनाएं।
2. Client ID को `upload.html` के `YOUR_GOOGLE_CLIENT_ID` और Worker variable `GOOGLE_CLIENT_ID` में रखें।
3. Authorized JavaScript origins में अपनी GitHub Pages origin जोड़ें।

## Cloudflare setup
1. R2 bucket: `samaras-mp-media`
2. KV namespace: `SAMARAS_MP_META`
3. Worker में bindings लगाएं।
4. `ADMIN_SECRET` को encrypted Worker Secret के रूप में रखें।
5. `wrangler.toml` में KV ID भरें या Dashboard bindings से configure करें।

## पहले test
1. Admin page से एक test uploader register करें।
2. उसकी Google email exactly वही रखें जिससे वह login करेगा।
3. 100 MB policy दें।
4. Upload page पर login करें।
5. पहले 1–2 MB फोटो test करें।
6. फिर 20–50 MB वीडियो।
7. उसके बाद क्षेत्र के अनुसार बड़ी file test करें।

## महत्वपूर्ण
यह MVP quota counter KV पर है। बहुत बड़े concurrent deployment में quota को Durable Object/D1 transaction से मजबूत करना चाहिए।
