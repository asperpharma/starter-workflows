# Asper Beauty Shop: System Monitor & Checklist

**Status:** Active
**Last Updated:** February 2026
**Purpose:** Central reference for monitoring the "Digital Concierge" ecosystem, from sales to system health.

---

## 1. Shopify Orders (The Vault)
*   **Admin Dashboard:** [Shopify Admin > Orders](https://admin.shopify.com/store/lovable-project-milns/orders)
*   **API Connection:**
    *   **Endpoint:** `https://lovable-project-milns.myshopify.com/api/2025-07/graphql.json`
    *   **Token Type:** `public_storefront_api_token` (Read-Only) [Source 2, 15]
    *   **Critical Scope:** `write_checkouts` (Ensures the cart can convert to an order) [Source 275].
*   **Verification:**
    *   Check for "Unfulfilled" orders with status "Payment Pending" (COD Orders).

## 2. Gorgias (The Concierge Interface)
*   **Dashboard:** [Gorgias Login](https://asperbeauty.gorgias.com) (Access via Shopify Apps)
*   **Ticket Views:**
    *   **"Medical/Product":** Questions triggered by `Concern_*` tags (handled by Dr. Sami persona).
    *   **"Support/Logistics":** Questions about "Shipping" or "COD" (Source 154, 295).
*   **Automation Check:**
    *   Verify the "Authenticity Guarantee" macro is active for keywords: `fake`, `original`, `real`.

## 3. Chat Logs (The Memory)
*   **Location:** Supabase Database
*   **Table:** `public.concierge_profiles` (and `consultations` if migrated) [Source 124, 175].
*   **What to Inspect:**
    *   `skin_concern`: The input tag (e.g., 'Acne', 'Dryness').
    *   `recommended_routine`: The JSON output containing specific SKUs (e.g., Vichy Normaderm).
*   **Access:** [Supabase Dashboard > Table Editor](https://supabase.com/dashboard/project/qqceibvalkoytafynwoc/editor)

## 4. Beauty Assistant Audit (The Intelligence)
*   **Table:** `public.admin_reports` (The "Daily Digest").
*   **Where to Inspect:**
    *   Go to **Supabase SQL Editor** or **Table Editor**.
    *   Query: `select * from admin_reports order by created_at desc;`
*   **Key Metrics:**
    *   `total_consultations`: Volume of users taking the quiz.
    *   `top_concern`: The trending skin issue (e.g., "Anti-Aging").

## 5. Health URL (The Pulse)
*   **Frontend Pulse:** `https://www.asperbeautyshop.com` (Verify Green SSL Padlock).
*   **Backend Brain Pulse:**
    *   **Endpoint:** `https://qqceibvalkoytafynwoc.supabase.co/functions/v1/beauty-assistant` [Source 63, 172].
    *   **Verification Command (Terminal):**
        ```bash
        curl -i --request POST 'https://qqceibvalkoytafynwoc.supabase.co/functions/v1/beauty-assistant' \
          --header 'Authorization: Bearer [YOUR_ANON_KEY]' \
          --header 'Content-Type: application/json' \
          --data '{"action": "health_check"}'
        ```
    *   **Expected Result:** HTTP 200 OK.

---

## 📋 Routine Checklists

### 🌅 Daily "Morning Rounds" (9:00 AM)
- [ ] **Orders:** Open Shopify Admin. Are there new COD orders? Mark as "Fulfilled" once shipped.
- [ ] **Gorgias:** Check "Unassigned" tickets. Did the AI escalate any medical issues?
- [ ] **Digest:** Check Supabase `admin_reports` for yesterday's top skin concern.

### 🗓️ Weekly "Clinical Audit" (Mondays)
- [ ] **Inventory Sync:** Run the `bulk-product-upload` function if you added new products to Shopify [Source 43].
- [ ] **System Pulse:** Run `./scripts/health-checks.ps1` in the terminal to verify build integrity.
- [ ] **Tagging:** Export a CSV from Matrixify and ensure all new items have `Concern_*` and `Step_*` tags [Source 132].
