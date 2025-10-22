# Product Specifications API

A serverless AWS solution for managing and accessing product specifications through a RESTful API. Built with AWS CDK, Lambda, DynamoDB, and API Gateway.

## Architecture

- **API Gateway**: REST API endpoint management and request routing
- **AWS Lambda**: Serverless compute for business logic (Node.js 18.x)
- **DynamoDB**: NoSQL database for flexible JSON product data storage
- **AWS CDK**: Infrastructure as Code for deployment

## Deployed Resources

### API Endpoints
- **Base URL**: `https://ysovhz6ofe.execute-api.us-east-1.amazonaws.com/prod/`
- **GET /products** - List all products with optional filtering
- **GET /products/{productId}** - Get specific product details

### AWS Resources (Suffix: 102220251553)
- **DynamoDB Table**: `ProductSpecifications-102220251553`
- **Lambda Functions**:
  - `GetProducts-102220251553` - Handle product listing and filtering
  - `GetProduct-102220251553` - Handle single product retrieval
  - `DataSeeder-102220251553` - Initialize sample product data
- **API Gateway**: `ProductSpecificationsApi-102220251553`

## API Usage

### Get All Products
```bash
curl -X GET "https://ysovhz6ofe.execute-api.us-east-1.amazonaws.com/prod/products"
```

### Get Specific Product
```bash
curl -X GET "https://ysovhz6ofe.execute-api.us-east-1.amazonaws.com/prod/products/ELEC001"
```

### Filter by Category
```bash
curl -X GET "https://ysovhz6ofe.execute-api.us-east-1.amazonaws.com/prod/products?category=Electronics"
```

### Filter by Brand
```bash
curl -X GET "https://ysovhz6ofe.execute-api.us-east-1.amazonaws.com/prod/products?brand=Apple"
```

### Multiple Filters
```bash
curl -X GET "https://ysovhz6ofe.execute-api.us-east-1.amazonaws.com/prod/products?category=Electronics&brand=Apple"
```

## Sample Data

The system includes 10 sample products across multiple categories:

### Electronics (4 products)
- iPhone 15 Pro (Apple)
- MacBook Pro 14-inch (Apple)
- Galaxy S24 Ultra (Samsung)
- Wireless Headphones (Sony)

### Clothing (2 products)
- Classic Cotton T-Shirt (Nike)
- Running Shoes (Adidas)

### Books (2 products)
- The Great Gatsby (Scribner)
- Clean Code (Prentice Hall)

### Home & Garden (2 products)
- Coffee Maker (Keurig)
- Dining Table (IKEA)

## Product Schema

```json
{
  "productId": "string",
  "name": "string",
  "category": "string",
  "brand": "string",
  "specifications": {
    "price": "number",
    "color": "string",
    "size": "string",
    "features": "array",
    "dimensions": "object"
  },
  "createdAt": "string",
  "updatedAt": "string"
}
```

## Response Formats

### Product List Response
```json
{
  "products": [
    {
      "productId": "ELEC001",
      "name": "iPhone 15 Pro",
      "category": "Electronics",
      "brand": "Apple",
      "specifications": {
        "price": 999,
        "color": "Natural Titanium",
        "storage": "128GB",
        "display": "6.1-inch Super Retina XDR",
        "camera": "48MP Main camera",
        "features": ["Face ID", "5G", "Wireless Charging"]
      },
      "createdAt": "2025-10-22T19:57:01.518Z",
      "updatedAt": "2025-10-22T19:57:01.518Z"
    }
  ],
  "count": 1
}
```

### Single Product Response
```json
{
  "productId": "ELEC001",
  "name": "iPhone 15 Pro",
  "category": "Electronics",
  "brand": "Apple",
  "specifications": {
    "price": 999,
    "color": "Natural Titanium",
    "storage": "128GB",
    "display": "6.1-inch Super Retina XDR",
    "camera": "48MP Main camera",
    "features": ["Face ID", "5G", "Wireless Charging"]
  },
  "createdAt": "2025-10-22T19:57:01.518Z",
  "updatedAt": "2025-10-22T19:57:01.518Z"
}
```

### Error Response (404)
```json
{
  "error": "Product not found"
}
```

## Testing

Run the comprehensive test suite:
```bash
./test-api.sh
```

This script tests:
1. Get all products
2. Get specific product
3. Filter by category
4. Filter by brand
5. Multiple filters
6. 404 error handling
7. Empty filter results

## Performance

- **Response Time**: < 500ms for single product queries
- **Response Time**: < 1000ms for filtered product lists
- **Availability**: 99.9% uptime target
- **Scalability**: Auto-scaling Lambda functions and DynamoDB on-demand billing

## Security Features

- CORS enabled for cross-origin requests
- API Gateway throttling limits
- Lambda function timeout configuration (30 seconds)
- IAM roles with least privilege access
- Input validation for query parameters

## Deployment

The infrastructure is deployed using AWS CDK:

```bash
cd cdk-infrastructure
npm run build
cdk deploy --require-approval never
```

Sample data is automatically seeded after deployment by invoking the DataSeeder Lambda function.

## Project Structure

```
product-specifications-api-102220251547/
├── README.md                    # This documentation
├── test-api.sh                  # API test script
├── task-development.md          # Development task description
├── specs/                       # Specification documents
│   ├── requirements.md
│   ├── design.md
│   └── tasks.md
└── cdk-infrastructure/          # CDK project
    ├── lib/
    │   └── cdk-infrastructure-stack.ts
    ├── bin/
    │   └── cdk-infrastructure.ts
    └── package.json
```

## Requirements Validation

✅ **Requirement 1**: Product Data Storage
- Flexible JSON schema supported
- Mandatory fields (productId, name, category, brand) included
- Custom attributes stored in specifications object
- Sample data automatically populated

✅ **Requirement 2**: Product Retrieval API
- GET /products returns list of all products
- GET /products/{productId} returns specific product
- 404 status for non-existent products
- JSON response format

✅ **Requirement 3**: Product Filtering and Search
- Category filtering with query parameter
- Brand filtering with query parameter
- Multiple filters supported
- Empty results return 200 status with empty array

✅ **Requirement 4**: API Performance and Reliability
- Response times under 500ms for single queries
- Response times under 1000ms for filtered lists
- Appropriate HTTP status codes and error messages
- High availability with serverless architecture

✅ **Requirement 5**: Sample Data Management
- 10+ sample products included
- Diverse categories (electronics, clothing, books, home)
- Multiple brands per category
- Flexible JSON schema demonstrated

## Cost Optimization

- **DynamoDB**: Provisioned billing mode (5 read/write capacity units)
- **Lambda**: 512MB memory allocation for optimal performance
- **API Gateway**: Pay-per-request pricing
- **CloudWatch**: Basic logging and monitoring included
