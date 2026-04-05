---
name: fb-messenger-reply
description: Reply to ALL unread Facebook Messenger messages for AI Ready Bangladesh page. Includes delivery tracking via Steadfast.
---

Reply to unread Facebook Messenger messages for the AI Ready Bangladesh page.

## Steps

1. Call `get_unread_conversations` with **limit 25** to catch all unread conversations (not just 5)
2. For each unread conversation, call `get_conversation_messages` to read the full context
3. **Skip these — do NOT reply:**
   - Emoji-only or sticker messages
   - Friend tags (peer-to-peer conversations where people are tagging each other)
   - Conversations where the last message is FROM the page (already replied)
4. For conversations that need a reply, craft a short casual Bangla reply following the rules below
5. Send the reply using `composio_send_messenger_reply` with the recipient's `from_id`
6. For damaged product replacements, notify Hiron via WhatsApp (see section below)

## Reply Rules

- **AI Ready Book:** 499 BDT, order at aiready.bd/books/ai-ready, delivery 60 BDT (Dhaka) / 90 BDT (outside Dhaka)
- **AI Literacy Micro Course:** 790 BDT (discounted from 1490), 29 lessons, 180 days access, enroll at aiready.bd/courses/aiready
- Keep replies **SHORT and conversational** like chat — not formal paragraphs
- Use **aiready.bd** domain (NOT aiready.com.bd)
- "Interested" or asks for details → share both products briefly
- Price/delivery question → answer specifically
- "Appointment" → clarify we sell books and courses, not appointments
- If customer confirms order or says thanks → acknowledge warmly
- Write in casual Bangla, matching the customer's tone

## Delivery Tracking — Steadfast Integration

When a customer asks about their order status, delivery status, tracking, or "কবে পাবো" / "কোথায় আছে" / "tracking" / "status":

1. **Extract their phone number** from the conversation (they may have shared it earlier, or ask for it)
2. Call `lookup_orders_by_phone` (aiready-convex) with their phone number to find their order
3. If the order has a `steadfastTrackingCode`:
   - Call `check_status_by_tracking_code` (aiready-steadfast) to get real-time status
   - Share the status in Bangla and include the tracking link: `https://steadfast.com.bd/t/{TRACKING_CODE}`
   - Example reply: "আপনার বই পাঠানো হয়েছে! Tracking code: {CODE}। স্ট্যাটাস: {STATUS}। ট্র্যাক করুন: https://steadfast.com.bd/t/{CODE}"
4. If the order has a `steadfastConsignmentId` but no tracking code yet:
   - Call `check_status_by_consignment_id` (aiready-steadfast) to check
   - Tell customer their order is being processed and they'll get tracking soon
5. If **no phone number available** in conversation:
   - Ask the customer for their phone number to check the order
   - Example: "অর্ডার চেক করতে আপনার ফোন নম্বরটা দেন প্লিজ"
6. If no order found for that phone:
   - Let them know politely and ask to double-check the number

**Steadfast status meanings (translate to casual Bangla):**
- `in_review` → "প্রসেসিং হচ্ছে"
- `pending` → "কুরিয়ারে হ্যান্ডওভার হয়েছে, ডেলিভারি হবে শীঘ্রই"
- `delivered` → "ডেলিভারি হয়ে গেছে"
- `cancelled` → "ক্যান্সেল হয়েছে"
- `hold` → "হোল্ডে আছে, কুরিয়ার যোগাযোগ করবে"
- `unknown` → "স্ট্যাটাস আপডেট পেন্ডিং"

## Damaged Book Replacement — Notify Hiron via WhatsApp

**Trigger:** A customer has confirmed they received a damaged/defective book AND has provided their name, address, and phone number for replacement.

When triggered:
1. Open WhatsApp desktop app using computer-use tools
2. Send a message to "Hiron Vai" (+880 1960-478935) with:
   - Customer name
   - Full address
   - Phone number
   - Issue description (e.g. torn cover, upside-down cover, etc.)
3. Use "apni" (not "tumi") when addressing Hiron — he is senior
4. Message format: "Hiron vai, ekta book replacement request ache — ektu handle korben apni please. [customer name, address, phone, issue]"

## Important
- Do NOT reply to messages that don't need a response
- If unsure about a message, skip it rather than sending a wrong reply
- Be helpful but concise — this is Messenger chat, not email
- Process ALL unread conversations, not just the most recent ones — customers should get a reply within the 24-hour Messenger window
