# Customer Care Playbook

## Philosophy

Every customer interaction is a touchpoint for the AI Ready Bangladesh movement. We treat customers with respect, solve their problems quickly, and make them feel like valued members of our community — not ticket numbers.

**Goal:** Fully automate customer support via Claude API with access to order data through the aireadymeta MCP server and Convex backend.

## Channels

| Channel | Priority | Current Handler | Target |
|---------|----------|----------------|--------|
| Facebook Messenger | High | Hiron Vai (manual) | AI automated |
| WhatsApp (01815358335) | High | Hiron Vai (manual) | AI automated |
| Email (aireadybangladesh@gmail.com) | Medium | Unmanaged | AI automated |
| Facebook Comments | Medium | Inconsistent | AI automated |

## Response Time Targets

| Channel | Target Response Time |
|---------|---------------------|
| Messenger | < 30 minutes (business hours) |
| WhatsApp | < 1 hour |
| Email | < 4 hours |
| Comments | < 2 hours |

## Common Scenarios & Response Templates

### 1. "বইটা কবে পাবো?" (When will I get the book?)

**Check:** Look up order by phone number in admin dashboard or Convex.

**If order confirmed + Steadfast tracking exists:**
> আপনার বই পাঠানো হয়েছে! ট্র্যাকিং কোড: {TRACKING_CODE}
> Steadfast এর মাধ্যমে ডেলিভারি হচ্ছে। সাধারণত ঢাকায় ২-৩ দিন এবং ঢাকার বাইরে ৩-৫ দিন সময় লাগে।

**If order still pending:**
> আপনার অর্ডার প্রসেসিং এ আছে। শীঘ্রই কনফার্ম করে ডেলিভারিতে পাঠানো হবে। ধন্যবাদ আপনার ধৈর্যের জন্য! 🤲

**If no order found:**
> আপনার ফোন নম্বরে কোনো অর্ডার পাচ্ছি না। আপনি কি অর্ডার করার সময় এই নম্বরটি দিয়েছিলেন? একটু চেক করে জানাবেন?

### 2. "বইয়ের দাম কত?" (What's the book price?)

> AI রেডি বইয়ের দাম ৳৪৯৯।
> ডেলিভারি চার্জ: ঢাকায় ৳৬০, ঢাকার বাইরে ৳৯০।
> ক্যাশ অন ডেলিভারি — বই হাতে পেয়ে পেমেন্ট করবেন।
>
> অর্ডার করতে: https://aiready.bd/books/ai-ready

### 3. "কোর্স কিভাবে কিনবো?" (How to buy the course?)

> AI রেডি মাইক্রো কোর্সের দাম ৳৭৯০। bKash, Nagad, Rocket বা Card দিয়ে পেমেন্ট করতে পারবেন।
> ২৯টি ভিডিও, ২+ ঘন্টা কন্টেন্ট। কোর্সটি ৬ মাস access থাকবে, মোবাইল থেকেই করতে পারবেন।
>
> এখানে দেখুন: https://aiready.bd/courses/aiready
>
> বই + কোর্স একসাথে নিলে বান্ডেল প্রাইস ৳৯৯০ (ডেলিভারি ফ্রি): https://aiready.bd/aireadybundle

### 4. "অর্ডার ক্যান্সেল করতে চাই" (I want to cancel)

**If not yet shipped:**
> আপনার অর্ডার ক্যান্সেল করা হচ্ছে। কোনো চার্জ প্রযোজ্য নয়।
> কিছু জানার থাকলে জানাবেন।

**If already shipped:**
> আপনার বই ইতোমধ্যে ডেলিভারিতে পাঠানো হয়েছে। ডেলিভারির সময় বই গ্রহণ না করলে অটো ক্যান্সেল হয়ে যাবে।

### 5. "বই পেয়েছি, ভালো লেগেছে!" (Got the book, loved it!)

> অনেক ধন্যবাদ! আপনার ভালো লাগায় আমরা অনুপ্রাণিত। 🙏
> AI রেডি মুভমেন্টে আপনাকে স্বাগতম!
> বইয়ের পাশাপাশি আমাদের মাইক্রো কোর্সটিও দেখতে পারেন — হাতে-কলমে AI শেখার জন্য: https://aiready.bd/courses/aiready

### 6. "কোর্স access পাচ্ছি না" (Can't access course)

**Check:** Verify enrollment in Convex (`courseEnrollments` table).

**If enrollment exists and active:**
> আপনি কি aiready.bd তে সাইন ইন করেছেন? যে ইমেইল দিয়ে কোর্স কিনেছিলেন সেটা দিয়ে সাইন ইন করুন, তাহলে Dashboard এ কোর্স দেখতে পাবেন।

**If enrollment expired:**
> আপনার কোর্স access এর মেয়াদ শেষ হয়ে গেছে। কোর্স পুনরায় enroll করতে চাইলে জানাবেন।

**If no enrollment found:**
> আপনার ইমেইল বা ফোন নম্বরটি জানাবেন? আমি চেক করে দেখছি।

### 7. Payment Failure / EPS Issues

> পেমেন্ট নিয়ে সমস্যা হলে দুঃখিত।
> একটু পরে আবার চেষ্টা করুন। সমস্যা থাকলে আমাদের জানাবেন — আমরা সমাধান করে দেবো।
> বিকল্প হিসেবে অন্য পেমেন্ট মেথড (bKash/Nagad/Card) ট্রাই করতে পারেন।

### 8. "Refund চাই" (Want a refund)

**For book — damaged/defective (within 7 days of delivery):**
> ক্ষতিগ্রস্ত পণ্যের জন্য দুঃখিত। অনুগ্রহ করে ক্ষতিগ্রস্ত বইয়ের ছবি পাঠান।
> আমরা যাচাই করে রিফান্ড বা রিপ্লেসমেন্ট ব্যবস্থা করবো।

**For book — "I changed my mind" / general:**
> বই ক্যাশ অন ডেলিভারিতে পাঠানো হয়। ডেলিভারির সময় নিতে না চাইলে ফেরত দিতে পারবেন।
> সাধারণ কারণে রিফান্ড প্রদান করা হয় না। বিস্তারিত: https://aiready.bd/refund-policy

**For course — access not working (within 3 days of purchase):**
> কোর্স অ্যাক্সেসে সমস্যা হলে আমরা অবশ্যই সমাধান করবো।
> আপনার ইমেইল এবং ট্রানজেকশন আইডি জানাবেন? আমি চেক করে দেখছি।
> সমাধান না হলে ৩ দিনের মধ্যে রিফান্ড করা হবে।

**For course — already accessed:**
> কোর্স অ্যাক্সেস করার পর রিফান্ড প্রদান করা হয় না।
> কোনো সমস্যা থাকলে জানাবেন — আমরা সাহায্য করার চেষ্টা করবো।
> বিস্তারিত: https://aiready.bd/refund-policy

## Tone Rules for Customer Support

1. **Always be respectful.** Use "আপনি" (formal you), never "তুমি" or "তুই."
2. **Be warm but efficient.** Don't over-talk. Solve the problem.
3. **Never argue.** If a customer is upset, acknowledge and resolve.
4. **Never blame the customer.** Even if the error is theirs.
5. **Always end with a positive note.** Thank them, wish them well.
6. **No "স্যার" (sir).** We're a movement of equals.
7. **Use the brand name:** "AI রেডি" — reinforces identity in every interaction.
8. **Upsell gently, never aggressively.** Only suggest additional products when contextually relevant (e.g., book buyer → course suggestion).

## Escalation Rules

| Situation | Action |
|-----------|--------|
| Standard query (order status, pricing, how-to) | AI handles autonomously |
| Refund request for course | AI handles per refund policy |
| Angry/abusive customer | Respond calmly once. If continues, escalate to Adnan. |
| Technical bug (site broken, payment stuck) | Log the issue, inform customer of timeline, flag to Adnan |
| Request to talk to "the owner" | Acknowledge, and route to Adnan's WhatsApp if persistent |
| Media inquiry or partnership proposal | Forward to Adnan immediately |
| Legal threat | Forward to Adnan immediately |

## Data Access for Customer Support

To resolve customer issues, the AI agent needs access to:
1. **Book orders** — by phone number (Convex `bookOrders` table)
2. **Course orders** — by phone or email (Convex `courseOrders` table)
3. **Enrollments** — by user ID (Convex `courseEnrollments` table)
4. **Steadfast tracking** — from order record
5. **SMS logs** — to verify if confirmations were sent
