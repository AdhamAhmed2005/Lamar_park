# Production Readiness – Bank Hosted Payments

## Current State (Dec 2, 2025)
- ✅ Live ARB credentials (.env) now use:
  - `ARB_TRANPORTAL_ID=eE3c9qOSg4Kl16W`
  - `ARB_TRANPORTAL_PASSWORD='2$H5P6LlU$jm5#b'`
  - `ARB_RESOURCE_KEY=53345272417553345272417553345272`
  - `ARB_TRANPORTAL_URL` / `ARB_PAYMENT_URL` → `https://digitalpayments.neoleap.com.sa/pg/payment/hosted.htm`
- ✅ Callback/base URLs updated to production domains:
  - `BACKEND_URL=https://api.lamarparks.com`
  - `ARB_RESPONSE_URL` + `ARB_ERROR_URL` → `/api/payment/response`
  - `FRONTEND_URL=https://lamarparks.com`
- ✅ `NODE_ENV=production` validation run via:
  - `NODE_ENV=production node -r dotenv/config config/validate.js --exit-on-error`
- ✅ Validator output: “Configuration validation completed successfully!”.

## Next Actions Before Go-Live
1. **Restart backend with fresh environment**
   ```bash
   cd backend
   npm run dev  # or npm start in production
   ```
2. **Run a live test transaction** once the server is deployed on the production domain.
3. **Confirm with Neoleap** after successful test so they can close out the go-live request.
4. **Monitor callbacks/logs** for the first few transactions; ensure `/api/payment/response` persists responses.
5. **Frontend**: verify redirects hit `https://lamarparks.com/payment/success|error`.

## Operational Notes
- Password contains `$` and `#`; keep the surrounding single quotes in `.env` so the full value loads.
- When running ad-hoc commands that `source .env`, remember to `unset ARB_TRANPORTAL_PASSWORD` afterward so dotenv can re-load it correctly.
- Supporting transactions (refund/void/inquiry) use `https://digitalpayments.neoleap.com.sa/pg/payment/tranportal.htm` if/when we implement them.

Everything is staged for production. Only remaining step is deploying the backend/frontend with this configuration and running the first live payment.
