# Coupon Swap (Full-Stack)

Full-stack app implementing:
- User Registration/Login (JWT)
- Add Coupon
- Browse/Search + Category filter
- Swap/Exchange (direct swap)
- Points/Credits (earn 1 credit when listing, spend 1 credit to buy)
- Buy/Sell (cash simulated)
- Verification & Reviews (endpoints + rating aggregation)
- Notifications/Reminders (cron logs for expiring coupons)

## Run Locally

### 1) Backend
```bash
cd backend
cp .env.sample .env  # update MONGO_URI if needed
npm install
npm run dev
```

### 2) Frontend
```bash
cd frontend
npm install
npm run dev
```
Open http://localhost:5173

> Frontend uses `VITE_API_URL` (default `http://localhost:5000/api`). To change:
```
# frontend/.env
VITE_API_URL=http://localhost:5000/api
```

## API Notes

- Register: `POST /api/auth/register { name, email, password }`
- Login: `POST /api/auth/login { email, password }`
- List coupons: `GET /api/coupons?q=nike&category=fashion`
- Create coupon: `POST /api/coupons` (auth) `{ brand,title,code,discount,price,category,description,expiry }`
- Verify coupon: `POST /api/coupons/:id/verify` (auth)
- Buy with credits: `POST /api/coupons/:id/buy/credits` (auth)
- Buy with cash: `POST /api/coupons/:id/buy/cash` (auth)

### Reviews
- Add: `POST /api/reviews` (auth) `{ coupon, rating (1..5), comment }`
- List: `GET /api/reviews?couponId=...`

### Swaps
- Create: `POST /api/swaps` (auth) `{ offeredCoupon, requestedCoupon }`
- My swaps: `GET /api/swaps` (auth)
- Respond: `POST /api/swaps/:id/respond` (auth) `{ decision: 'accepted'|'rejected' }`

## Notes
- Email notifications are simulated via console logs at 9:00 IST daily using `node-cron`.
- Payment is simulated. For real payments, integrate Stripe/Razorpay.
- Credits: +1 credit on signup and +1 for each listed coupon; spend 1 credit to buy a coupon with credits.
- For verification, you may add admin-only checks later.
