# MCP Spring Boot Product Manager

A Model Context Protocol (MCP) server that integrates with a Spring Boot application to provide AI-powered product management through natural language commands.

## 🚀 Overview

This project demonstrates how to integrate AI capabilities into a Spring Boot & Angular application using MCP (Model Context Protocol). Users can manage products using natural language commands like:

- "Add a product called 'Paper Tissues' with price 500 and availability 30"
- "Delete the product 'Paper Tissues'"
- "Update the price of 'Paper Tissues' to 450"
- "Show me all products"

## 🏗️ Architecture

```
User Input (Natural Language)
         ↓
    Claude AI Assistant
         ↓
    MCP Server (Python)
         ↓
    Spring Boot API
         ↓
    Database (H2/PostgreSQL)
```

## 📋 Prerequisites

- **Java 17+**
- **Maven 3.6+**
- **Python 3.8+**
- **Node.js 16+** (for Angular frontend)
- **Claude AI access** (via Anthropic API or Claude Desktop)

## 🛠️ Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/mcp-springboot-product-manager.git
cd mcp-springboot-product-manager
```

### 2. Set up Spring Boot Backend

```bash
cd backend
mvn clean install
```

### 3. Set up MCP Server

```bash
cd mcp
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 4. Set up Angular Frontend (Optional)

```bash
cd frontend
npm install
```

## 🔧 Configuration

### Spring Boot Configuration

Create `application.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.h2.console.enabled=true

# JPA Configuration
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create-drop

# Server Configuration
server.port=8080

# CORS Configuration
spring.web.cors.allowed-origins=*
spring.web.cors.allowed-methods=GET,POST,PUT,DELETE,OPTIONS
spring.web.cors.allowed-headers=*
```

### MCP Server Configuration

Update the `SPRING_BOOT_BASE_URL` in `mcp/product_mcp_server.py`:

```python
SPRING_BOOT_BASE_URL = "http://localhost:8080/api/products"
```

### Claude Desktop Configuration

Add to your Claude Desktop configuration file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "product-manager": {
      "command": "python",
      "args": ["/path/to/your/mcp/product_mcp_server.py"]
    }
  }
}
```

## 🚀 Running the Application

### 1. Start Spring Boot Backend

```bash
cd backend
mvn spring-boot:run
```

The API will be available at `http://localhost:8080`

### 2. Start MCP Server

```bash
cd mcp
python product_mcp_server.py
```

### 3. Start Angular Frontend (Optional)

```bash
cd frontend
ng serve
```

The frontend will be available at `http://localhost:4200`

### 4. Test with Claude

Open Claude Desktop and try these commands:

```
Add a product called "Laptop" with price 999.99 and availability 10
```

```
List all products
```

```
Delete the product "Laptop"
```

## 🔧 API Endpoints

### Spring Boot REST API

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/products` | Get all products |
| GET | `/api/products/{id}` | Get product by ID |
| GET | `/api/products/search?q={query}` | Search products |
| POST | `/api/products` | Create new product |
| PUT | `/api/products/{id}` | Update product |
| DELETE | `/api/products/{id}` | Delete product |

### MCP Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `add_product` | Add a new product | name, price, availability, description |
| `delete_product` | Delete a product by name | name |
| `update_product` | Update product details | name, new_price, new_availability, new_description |
| `list_products` | List all products | limit |
| `search_products` | Search products by name | query |

## 📊 Database Schema

### Product Entity

```sql
CREATE TABLE products (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    availability INT NOT NULL,
    description TEXT
);
```

## 🧪 Testing

### Unit Tests (Spring Boot)

```bash
cd backend
mvn test
```

### Integration Tests (MCP)

```bash
cd mcp
python -m pytest tests/
```

### Manual Testing

Test the MCP server directly:

```bash
cd mcp
python test_mcp_server.py
```

## 📝 Example Usage

### Natural Language Commands

```bash
# Adding products
"Add a product called 'MacBook Pro' with price 2499 and availability 5"
"Create a new item named 'iPhone 15' priced at 999 with 20 units available"

# Updating products
"Update the price of 'MacBook Pro' to 2299"
"Change the availability of 'iPhone 15' to 15"

# Deleting products
"Remove the product 'MacBook Pro'"
"Delete the item named 'iPhone 15'"

# Querying products
"Show me all products"
"List the first 5 products"
"Search for products containing 'phone'"
```

### API Responses

```json
{
  "id": 1,
  "name": "MacBook Pro",
  "price": 2499.0,
  "availability": 5,
  "description": "High-performance laptop"
}
```

## 🔒 Security Considerations

### For Production Use:

1. **Database Security**
   - Use PostgreSQL/MySQL instead of H2
   - Configure proper database credentials
   - Enable SSL connections

2. **API Security**
   - Implement JWT authentication
   - Add rate limiting
   - Configure proper CORS origins
   - Input validation and sanitization

3. **MCP Security**
   - Implement authentication for MCP server
   - Add request logging and monitoring
   - Configure proper error handling

## 🐛 Troubleshooting

### Common Issues

1. **Connection Refused Error**
   ```
   Solution: Ensure Spring Boot is running on port 8080
   ```

2. **CORS Error**
   ```
   Solution: Add @CrossOrigin annotation to controllers
   ```

3. **MCP Server Not Found**
   ```
   Solution: Check the path in Claude Desktop configuration
   ```

4. **Database Connection Issues**
   ```
   Solution: Verify H2 configuration in application.properties
   ```

### Debug Mode

Enable debug logging:

```python
# In product_mcp_server.py
import logging
logging.basicConfig(level=logging.DEBUG)
```

## 📚 Dependencies

### Spring Boot (Maven)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

### MCP Server (Python)

```txt
mcp>=1.0.0
aiohttp>=3.8.0
asyncio
```

### Angular Frontend (package.json)

```json
{
  "dependencies": {
    "@angular/core": "^17.0.0",
    "@angular/common": "^17.0.0",
    "@angular/forms": "^17.0.0",
    "@angular/router": "^17.0.0"
  }
}
```

## 🚀 Deployment

### Docker Deployment

```dockerfile
# Dockerfile for Spring Boot
FROM openjdk:17-jdk-slim
COPY target/product-manager-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

```dockerfile
# Dockerfile for MCP Server
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY product_mcp_server.py .
CMD ["python", "product_mcp_server.py"]
```

### Docker Compose

```yaml
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "8080:8080"
    environment:
      - SPRING_PROFILES_ACTIVE=prod
  
  mcp-server:
    build: ./mcp
    depends_on:
      - backend
    environment:
      - SPRING_BOOT_BASE_URL=http://backend:8080/api/products
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Links

- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Angular Documentation](https://angular.io/)
  
## 📞 Support

For support, please open an issue on GitHub or contact [your-email@example.com](mailto:your-email@example.com).

## 🎯 Roadmap

- [ ] Add user authentication
- [ ] Implement product categories
- [ ] Add inventory tracking
- [ ] Create admin dashboard
- [ ] Add email notifications
- [ ] Implement product images
- [ ] Add order management
- [ ] Create reporting features

---

**Made with ❤️ by [Ben Youssef Wael]**
