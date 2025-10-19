# SpaceRemit Payment Gateway for WooCommerce

A comprehensive WordPress plugin that integrates SpaceRemit payment gateway with WooCommerce, featuring a complete transaction management dashboard and High-Performance Order Storage (HPOS) compatibility.

## Features

- **Complete WooCommerce Integration**: Seamless payment processing through SpaceRemit API
- **HPOS Compatibility**: Fully compatible with WooCommerce High-Performance Order Storage
- **Transaction Management Dashboard**: Comprehensive admin interface for monitoring all transactions
- **Real-time Status Updates**: Webhook support for instant payment status updates
- **Refund Support**: Process refunds directly through the admin interface
- **Security**: Proper webhook signature validation and secure API communication
- **Responsive Design**: Mobile-friendly admin interface
- **Export Functionality**: Export transaction data for reporting

## Installation

1. **Upload the Plugin**:
   - Download or clone this repository
   - Upload the entire `spaceremit-payment-gateway` folder to `/wp-content/plugins/`
   - Or upload the zip file through WordPress admin panel

2. **Activate the Plugin**:
   - Go to WordPress Admin → Plugins
   - Find "SpaceRemit Payment Gateway" and click "Activate"

3. **Configure the Gateway**:
   - Go to WooCommerce → Settings → Payments
   - Find "SpaceRemit" and click "Set up"
   - Enter your API credentials from SpaceRemit dashboard

## Configuration

### API Settings

1. **Test Mode**: Enable for testing with sandbox credentials
2. **API Keys**: 
   - Get your API keys from [SpaceRemit Dashboard](https://dashboard.spaceremit.com)
   - Use test keys for sandbox mode
   - Use live keys for production

### Webhook Setup

1. In your SpaceRemit dashboard, set the webhook URL to:
   ```
   https://yoursite.com/wc-api/spaceremit_webhook
   ```

2. Configure webhook events:
   - Payment completed
   - Payment failed
   - Payment cancelled
   - Refund processed

## Usage

### For Customers

1. During checkout, select "SpaceRemit" as payment method
2. Click "Place Order" to be redirected to SpaceRemit payment page
3. Complete payment using preferred method
4. Return to your site with payment confirmation

### For Administrators

#### Dashboard Overview

- Navigate to **SpaceRemit → Dashboard** for overview
- View transaction statistics and recent activity
- Monitor payment success rates and amounts

#### Transaction Management

- Go to **SpaceRemit → Transactions** for detailed view
- Filter transactions by status, date, or search terms
- View individual transaction details
- Process refunds when needed

#### Settings

- Access **SpaceRemit → Settings** for quick navigation to payment settings
- Or go directly to **WooCommerce → Settings → Payments → SpaceRemit**

## API Integration

This plugin integrates with SpaceRemit API v2 following the official documentation:
- Base URL: `https://api.spaceremit.com/v2` (Live)
- Sandbox URL: `https://sandbox-api.spaceremit.com/v2` (Test)

### Supported Operations

- **Create Payment**: Initialize payment transactions
- **Verify Payment**: Confirm payment status
- **Process Refunds**: Handle refund requests
- **Get Exchange Rates**: Retrieve current rates
- **Webhook Validation**: Secure webhook processing

## Database Schema

The plugin creates a custom table `wp_spaceremit_transactions` with the following structure:

```sql
CREATE TABLE wp_spaceremit_transactions (
    id mediumint(9) NOT NULL AUTO_INCREMENT,
    order_id bigint(20) NOT NULL,
    transaction_id varchar(100) NOT NULL,
    amount decimal(10,2) NOT NULL,
    currency varchar(3) NOT NULL,
    status varchar(20) NOT NULL DEFAULT 'pending',
    gateway_response longtext,
    created_at datetime DEFAULT CURRENT_TIMESTAMP,
    updated_at datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (id),
    UNIQUE KEY transaction_id (transaction_id),
    KEY order_id (order_id),
    KEY status (status)
);
```

## HPOS Compatibility

This plugin is fully compatible with WooCommerce's High-Performance Order Storage (HPOS):

- Declares compatibility using `FeaturesUtil::declare_compatibility()`
- Uses WooCommerce order objects instead of direct post meta access
- Follows HPOS best practices for order data handling

## Security Features

- **API Authentication**: Secure Bearer token authentication
- **Webhook Validation**: HMAC signature verification
- **Data Sanitization**: All inputs properly sanitized and validated
- **Nonce Protection**: WordPress nonces for AJAX requests
- **SQL Injection Prevention**: Prepared statements for all database queries

## Troubleshooting

### Common Issues

1. **Payment Not Processing**:
   - Check API credentials in settings
   - Verify webhook URL is accessible
   - Check error logs in WooCommerce → Status → Logs

2. **Webhook Not Working**:
   - Ensure webhook URL is correctly configured in SpaceRemit dashboard
   - Check server logs for incoming webhook requests
   - Verify SSL certificate is valid

3. **Transaction Status Not Updating**:
   - Check webhook signature validation
   - Verify transaction exists in database
   - Check WooCommerce order status

### Debug Mode

Enable WordPress debug mode by adding to `wp-config.php`:

```php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
```

Check logs at `/wp-content/debug.log`

## Requirements

- WordPress 5.0 or higher
- WooCommerce 5.0 or higher
- PHP 7.4 or higher
- SSL certificate (required for webhooks)
- SpaceRemit merchant account

## Support

For technical support:

1. Check the [SpaceRemit API Documentation](https://spaceremit.com/apiinfo-v2)
2. Review WooCommerce payment gateway guidelines
3. Contact SpaceRemit support for API-related issues

## License

This plugin is licensed under GPL v2 or later.

## Changelog

### Version 1.0.0
- Initial release
- Complete SpaceRemit API integration
- Transaction management dashboard
- HPOS compatibility
- Webhook support
- Refund functionality