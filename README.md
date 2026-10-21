# SKU.io MCP Server

The official [Model Context Protocol](https://modelcontextprotocol.io) server for [SKU.io](https://www.sku.io), the inventory and order management system for multi-channel e-commerce.

Connect an AI assistant to your SKU.io account and it can look up products, stock per warehouse, sales orders and purchase orders. If you allow it when you connect, it can also create draft sales orders and purchase orders, adjust inventory, record supplier invoices and more.

This is a hosted (remote) server. There is nothing to install or run. This repository holds its documentation and its registry entry (`server.json`), not its source code.

| | |
|---|---|
| Server URL | `https://mcp.sku.io/mcp` |
| Transport | Streamable HTTP |
| Authentication | OAuth 2.1 with PKCE (dynamic client registration and client ID metadata documents are both supported) |
| Registry name | `io.sku/sku` |

Every SKU.io account uses the same URL. You choose your account when you sign in.

## Requirements

- A SKU.io account. The assistant sees only what your role can see, and changes only what your role can change.
- An admin can switch assistants off for the whole account, or limit them to looking things up, under **Settings → Developer → MCP Server → Settings**.

## Configure

### VS Code (GitHub Copilot)

Install it from the MCP server gallery (search `@mcp SKU.io` in the Extensions view), or add it to `.vscode/mcp.json`:

```json
{
  "servers": {
    "sku": {
      "type": "http",
      "url": "https://mcp.sku.io/mcp"
    }
  }
}
```

Start the server, and VS Code opens a browser window for you to sign in to SKU.io.

### Claude, ChatGPT and Cursor

Add a custom connector with the URL `https://mcp.sku.io/mcp` and leave its sign-in set to OAuth. Step-by-step instructions for each app: [Connect an AI assistant](https://www.sku.io/docs/guides/settings/developer/apps-and-webhooks/connect-an-ai-assistant/).

### Claude Code

```bash
claude mcp add --transport http sku https://mcp.sku.io/mcp
```

Then run `/mcp` in Claude Code and choose **Authenticate**.

## Sign in and choose what the assistant may do

When your client sends you to SKU.io:

1. Check that **You will be sent back to** names the app you started from. If it names anything else, click **Cancel**.
2. Choose the SKU.io **Account** to connect, if you belong to more than one.
3. **View data** is always on. To let the assistant create and update records, turn on **Allow changes**, then untick any area it should not change: Orders, Purchasing, Inventory, Products, Accounting, or Settings & automations.
4. Click **Approve**.

## What it can do

| Access | Tools |
|---|---|
| View data (always) | `find_product`, `search_products`, `get_products`, `get_inventory_levels`, `get_stockout_risk`, `list_warehouses`, `get_orders`, `get_order_status`, `search_purchase_orders`, `get_purchase_order_details`, `get_send_plan`, `explain_send`, `compare_scenarios`, `get_reimbursement_cases`, `diagnose_tiktok_listing`, `list_support_insights_reports`, `get_support_insights_report`, `whoami` |
| Orders | `create_sales_order` (always a draft until you approve it in SKU.io) |
| Purchasing | `create_purchase_order` (draft), `submit_purchase_order`, `record_purchase_invoice` |
| Inventory | `adjust_inventory` |
| Products | `create_product`, `update_product_pricing` |
| Accounting | `record_vendor_credit` (draft) |
| Settings & automations | `create_saved_view`, `create_workflow_draft` (never published) |

Four more tools reach the rest of the [SKU.io REST API](https://developer.sku.io): `search_api` and `describe_endpoint` find and explain an endpoint, `api_read` calls read-only endpoints, and `api_write` runs a fixed set of changes SKU.io has reviewed for assistants. Each change still needs the matching **Allow changes** area.

Whatever you approve, an assistant can never delete or void anything, or manage users, roles, billing or tokens. Every call it makes is listed on the **Activity** tab under **Settings → Developer → MCP Server**.

## Example prompts

- "How much stock of the 750 ml water bottle do we have, and is more on order?"
- "Which products will run out in the next 30 days?"
- "What's the status of order 10482?"
- "Draft a purchase order to our usual supplier for everything at risk of stocking out."

Each answer links back to the record in SKU.io.

## Links

- Setup guide: https://www.sku.io/docs/guides/settings/developer/apps-and-webhooks/connect-an-ai-assistant/
- API reference: https://developer.sku.io
- Agent skills for SKU.io: https://github.com/skuio/sku-skills
- Privacy policy: https://www.sku.io/privacy-policy/
- Support: support@sku.io
