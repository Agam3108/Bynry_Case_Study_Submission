# Bynry_Case_Study_Submission
Case Study Submission for Backend Engineering Intern at Bynry
# Inventory Management System - Case Study

A comprehensive case study covering code review, database design, and API implementation for a multi-warehouse inventory management system.

## 📋 Table of Contents

- [Overview](#overview)
- [Case Study Parts](#case-study-parts)
- [Technical Stack](#technical-stack)
- [Database Schema](#database-schema)
- [API Documentation](#api-documentation)
- [Setup Instructions](#setup-instructions)
- [Key Highlights](#key-highlights)

## 🎯 Overview

This case study demonstrates expertise in:
- **Code Review & Refactoring**: Identifying and fixing production issues in Flask APIs
- **Database Design**: Designing normalized schemas for complex inventory systems
- **API Development**: Building robust, production-ready REST APIs
- **Error Handling**: Comprehensive edge case management
- **Performance Optimization**: Indexing strategies and query optimization

## 📚 Case Study Parts

### Part 1: Code Review & Refactoring
[View Documentation](docs/01-code-review.md)

**Scenario**: Review and fix a broken product creation endpoint

**Issues Identified & Fixed**:
- ✅ Input validation (preventing 500 errors)
- ✅ SKU uniqueness constraints
- ✅ Data type validation (Decimal for prices)
- ✅ Transaction rollback handling
- ✅ Warehouse existence validation
- ✅ Duplicate inventory prevention

---

**Scenario**: Design a scalable database for multi-warehouse inventory management

**Key Features**:
- Multi-company and multi-warehouse support
- Product bundles (products containing other products)
- Inventory tracking with transaction history
- Supplier management
- Warehouse transfers


**Entity Relationship Diagram**:
![ERD](docs/architecture-diagram.png)

---

### Part 3: API Implementation
[View Documentation](docs/03-api-implementation.md)

**Scenario**: Build a low-stock alerts endpoint

**Endpoint**: `GET /api/companies/{company_id}/alerts/low-stock`

**Business Logic**:
- Calculate days until stockout based on sales velocity
- Filter products with recent sales activity
- Include preferred supplier information
- Support multiple warehouses per company

---

## 🛠 Technical Stack

- **Backend**: Python 3.9+, Flask 2.3+
- **Database**: PostgreSQL 14+
- **ORM**: SQLAlchemy 2.0+
- **Testing**: pytest
- **Documentation**: Markdown

## 📊 Database Schema
```
COMPANIES (1:M) ──── WAREHOUSES
    │
    ├── (1:M) PRODUCTS ──── (M:M) PRODUCT_BUNDLES (self-referencing)
    │                  │
    │                  └── (1:M) INVENTORY ──── (1:M) INVENTORY_TRANSACTIONS
    │                               │
    │                               └── (M:1) WAREHOUSES
    │
    └── (M:M) ──── SUPPLIERS ──── (M:M) PRODUCTS
                                      (via PRODUCT_SUPPLIERS)
```

**Key Tables**: 13 tables with proper indexing and constraints

## 🔌 API Documentation

### Create Product
```http
POST /api/products
Content-Type: application/json

{
  "name": "Widget A",
  "sku": "WID-001",
  "price": 19.99,
  "warehouse_id": 1,
  "initial_quantity": 100
}
```

### Get Low-Stock Alerts
```http
GET /api/companies/1/alerts/low-stock?days_lookback=90

Response:
{
  "alerts": [
    {
      "product_id": 123,
      "product_name": "Widget A",
      "current_stock": 5,
      "threshold": 20,
      "days_until_stockout": 12,
      "supplier": {...}
    }
  ],
  "total_alerts": 1
}
```

### Code Quality
- ✅ Comprehensive input validation
- ✅ Proper error handling with rollback
- ✅ Type safety (Decimal for monetary values)
- ✅ SQL injection prevention
- ✅ Detailed logging

### Database Design
- ✅ Normalized to 3NF
- ✅ Strategic indexing for performance
- ✅ Audit trail with transaction history
- ✅ Referential integrity with foreign keys
- ✅ Check constraints for data validation

### API Design
- ✅ RESTful conventions
- ✅ Proper HTTP status codes
- ✅ Edge case handling (14+ scenarios)
- ✅ Sales velocity calculations
- ✅ Flexible filtering options

## 📝 Design Decisions

### Why Separate inventory and inventory_transactions Tables?
- **inventory**: Current state (fast reads for "what's in stock now?")
- **inventory_transactions**: Immutable audit log (historical tracking)
- Enables efficient queries without complex aggregations

### Why Decimal for Prices?
- Floating-point arithmetic causes rounding errors in financial calculations
- Decimal provides exact precision for monetary values
- Example: `0.1 + 0.2 = 0.30000000000000004` (float) vs `0.30` (Decimal)

### Why Single Transaction with flush()?
- Prevents orphaned products if inventory creation fails
- Atomic operation ensures data consistency
- Rollback on any error maintains database integrity

## 📄 License

This project is for educational and portfolio purposes.

## 👤 Author

**Your Name**
- GitHub: [@Agam3108](https://github.com/Agam3108)
- LinkedIn: [Agam318](https://www.linkedin.com/in/agam318/)
- Email: your.email@example.com

## 🙏 Acknowledgments

This case study demonstrates real-world software engineering practices including:
- Production-ready error handling
- Scalable database design
- Performance optimization strategies
- Comprehensive documentation

---

**Note**: This is a case study project demonstrating technical skills. For production use, additional features like authentication, rate limiting, and monitoring would be required.
