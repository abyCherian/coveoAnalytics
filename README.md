# Purchase Event Tracking for Coveo Analytics in Google Tag Manager

## Overview
This guide explains how to implement purchase event tracking for **Coveo Analytics** using **Google Tag Manager (GTM)**. The implementation ensures that eCommerce transactions are properly recorded in Coveo's analytics platform.

## Prerequisites
- **Google Tag Manager (GTM)** access
- **Coveo Analytics API Key** with necessary permissions
- **Data Layer Implementation** with purchase event details
- Basic knowledge of JavaScript and GTM

## Data Layer Structure
Ensure the following data is pushed to the **Data Layer** when a purchase event occurs:

```javascript
window.dataLayer.push({
  event: "purchase",
  transactionId: "12345",
  currency: "USD",
  revenue: 100.0,
  items: [
    {
      id: "product_001",
      name: "Product Name",
      category: "Category Name",
      price: 50.0,
      quantity: 2
    }
  ]
});
```

## Step-by-Step Implementation in GTM

### 1. Create Variables
#### a) Data Layer Variables
- **Transaction ID:** `{{DLV - transactionId}}`
- **Currency:** `{{DLV - currency}}`
- **Revenue:** `{{DLV - revenue}}`
- **Items:** `{{DLV - items}}`

### 2. Create a Custom HTML Tag
1. **Tag Type:** Custom HTML
2. **Tag Name:** `Coveo - Purchase Event`
3. **Code Snippet:**

```html
<script>
(function() {
  var purchaseData = {
    name: "purchase",
    type: "event",
    value: {{DLV - revenue}},
    currency: "{{DLV - currency}}",
    customData: {
      transactionId: "{{DLV - transactionId}}",
      items: {{DLV - items}}
    }
  };
  
  if (window.coveoua) {
    window.coveoua('send', purchaseData);
  } else {
    console.warn("Coveo Analytics library not found.");
  }
})();
</script>
```

4. **Trigger:** Select **purchase event** trigger.

### 3. Publish and Test
1. **Enable GTM Preview Mode** and trigger a test purchase.
2. **Verify network requests** using the browser developer console (`Network` tab → look for Coveo API calls).
3. **Check Coveo Analytics reports** to confirm event tracking.

## Troubleshooting
- Ensure the **Coveo Analytics script** is loaded before sending events.
- Use **console logs** to debug missing or incorrect data.
- Validate **Data Layer values** using GTM Preview mode.

## References
- [Coveo Analytics API Documentation](https://docs.coveo.com/en)
- [Google Tag Manager Guide](https://support.google.com/tagmanager)

---
**Author:** Abilash Cherian  
**Last Updated:** March 2025
