# Document Management System

This repository serves as a guide and technical specifications document for developing the Document Management System Project. Read it carefully to understand the full specifications for the project.

## Table of Contents

1. [Project Summary](#project-summary)
    - [Project Overview](#project-overview)
    - [Scope](#scope)
    - [Users, Roles, and Workflows](#users-roles-and-workflows)
        - [System Administrator (Admin)](#system-administrator-admin)
        - [Supplier](#supplier)
2. [System Architecture](#system-architecture)
    - [Technology Stack](#technology-stack)
    - [Main Components](#main-components)
3. [Development Environment](#development-environment)
    - [GitHub Repository Structure](#github-repository-structure)
    - [Collaboration Guidelines](#collaboration-guidelines)
4. [Development Workflow](#development-workflow)
    - [Local Development Setup](#local-development-setup)
5. [Implementation Phases](#implementation-phases)
    - [Phase 1: Core System (Week 1)](#phase-1-core-system-week-1)
    - [Phase 2: Frontend (Weeks 2-3)](#phase-2-frontend-weeks-2-3)
    - [Phase 3: Document System (Week 4)](#phase-3-document-system-week-4)
    - [Phase 4: Completion (Weeks 5-6)](#phase-4-completion-weeks-5-6)
6. [Deployment](#deployment)
    - [Initial Deployment (Local)](#initial-deployment-local)
    - [Future Infrastructure Integration](#future-infrastructure-integration)
7. [Documentation](#documentation)
    - [Code Documentation](#code-documentation)
    - [User Documentation](#user-documentation)
8. [Contact Information](#contact-information)

---

## Project Summary

### Project Overview

Develop a **document management system** for **CUSTOMER** to replace the current third-party application. The new system aims to enhance documentation management between property administrators, homeowner communities, and service providers.

**Business Logic:**

- **Administrator:** CUSTOMER and its staff.
- **Client:** Typically a Management Firm or Property Administrator managing multiple Homeowner Communities. In practice, this role is also performed by CUSTOMER.
- **Homeowner Community:** Represented as a WorkCenter within the platform. They do not have direct access.
- **Suppliers:** Companies providing services to each Community. They have direct access to the platform to submit documentation.

### Scope

- **Web-Based Document Management System:**
  - Document validation, tracking, and approval.
- **User and Role Management:**
  - Two main roles with distinct user experiences:
    - **Provider Portal**
    - **Admin/Manager Dashboard**
- **Automated Notification System:**
  - Periodic notifications for pending document approvals.
- **Development:**
  - **Backend:** [See Backend Specifications](#)
  - **Frontend:** Utilizing AdminLTE ([See Frontend Specifications](#))

### Users, Roles, and Workflows

Each user has a tailored workflow based on their role:

#### System Administrator (Admin)

**Description:**
- Full access to the application without restrictions.
- Represents CUSTOMER and its staff.

**Key Actions:**

1. **Register Management Firms / Property Administrators:**
   - Create new organizations with `organization_type = "CLIENT"`.
   - *Example:* `gestor@gestoriagonzalez.com`

2. **Manage Communities:**
   - Create and modify WorkCenters managed by specific Clients.
   - *Example WorkCenters:*
     - “Homeowner Community Calle Mayor 15” (WorkCenter #1)
     - “Community Edificio Sol” (WorkCenter #2)

3. **Register Suppliers and Assign Jobs:**
   - Assign Suppliers to specific Communities.

4. **Invite or Register New Suppliers:**
   - Create new organizations with `organization_type = "PROVIDER"`.
   - Alternatively, associate existing Suppliers from the database.

5. **Create Provider Users:**
   - Assign users with the role `Provider` to each Supplier.
   - Users are invited via email and have restricted read permissions related to their assigned provider.

6. **Register Jobs for WorkCenters:**
   - Assign works to Providers within specific WorkCenters.

7. **Manage Document Requests:**
   - Associate `DocumentRequests` to specific jobs.
   - Validate documents uploaded by Providers.

#### Supplier

**Description:**
- Limited permissions focused on their organization’s data.

**Key Actions:**

1. **Access the Platform:**
   - Log in after registration or invitation.

2. **Review Document Requests:**
   - View `DocumentRequests` from various Communities (WorkCenters).
   - Suppliers can serve multiple clients.

3. **Upload Documents:**
   - Upload required documents (e.g., PDF of Liability Insurance) for requests with a “Pending” status.
   - Status updates to “Under Review” upon upload.

4. **Receive Notifications:**
   - Alerts for rejected documents with the option to upload corrections.
   - Periodic summaries of pending actions.

---

## System Architecture

### Technology Stack

**Initial Development:**
- **Backend:** Django
- **Database:** PostgreSQL
- **Frontend:** AdminLTE, HTML5, CSS3, JavaScript
- **Infrastructure:**
  - Local storage for initial development
  - Local email handling

**Production Deployment:**
- Deploy on the **OASIX ecosystem**.

### Main Components

1. **Authentication and Authorization System**
   - User management
   - Role-based access control
   - Session handling using Django's default authentication system
   - **Session Timeout:** 8 hours
   - **Password Requirements:**
     - Minimum 8 characters
     - Includes numbers and letters
     - At least one uppercase letter

2. **Document Management**
   - Upload and storage
   - Version control
   - Status tracking
   - Validation workflow
   - **File Specifications:**
     - Max size: 20MB
     - Allowed formats: PDF, DOC, DOCX, XLS, XLSX, JPG, PNG
   - **Storage Structure:** `/documents/{organization_id}/{year}/{month}/`
   - **File Naming Convention:** `{uuid}_{original_filename}` to prevent naming conflicts

3. **Notification System**
   - Email notifications for status updates
   - Reminder system for pending actions

4. **Administrative Panel**
   - User management
   - System configuration
   - Audit logs

---

## Development Environment

### GitHub Repository Structure

- **Repository Name:** `CUSTOMER-document-management`
- **Main Branches:**
  - `main`: Production code
  - `develop`: Development branch

### Collaboration Guidelines

- **Commit Messages:** Must be in English.
- **Branching Strategy:** Follow [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/) for managing features, releases, and hotfixes.
- **Code Reviews:** Mandatory for all pull requests to ensure code quality and consistency.

---

## Development Workflow

### Local Development Setup

Follow these steps to set up the development environment locally:

```bash
# Clone the repository
git clone https://github.com/organization/CUSTOMER-document-management.git

# Navigate to the project directory
cd CUSTOMER-document-management

# Create a virtual environment
python -m venv venv

# Activate the virtual environment
# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Apply database migrations
python manage.py migrate

# Start the development server
python manage.py runserver
```

## Implementation Phases

### Phase 1: Core System (Weeks 1)

- Project setup
- Basic models implementation
- Authentication system
- CRUD APIs
- Local storage implementation

Deliverable: System administration demo. User, records creation through the django admin panel.

### Phase 2: Frontend (Weeks 2-3)
- Basic frontend development using AdminLTE
- Core forms
- Navigation
- Backend integration

Deliverable: Navigation through the app demo. Main views and forms for each role developed.

### Phase 3: Document System (Week 4)
- Document upload
- Status management
- Search system
- Email notifications

Deliverable: Documental system validation demo.

### Phase 4: Completion (Weeks 5-6)
- Dashboard and reports
- Testing and fixes
- Documentation
- Production deployment

Deliverable: Full App and Deployment in OASIX.

## Deployment

### Initial Deployment (LOCAL)
- Local file storage (Use Django abstraction layer for avoiding issues when changing to OASIX)
- Local email handling
- PostgreSQL database (Will be Provided)
- Django application server (Until deployed, use local)
- Any more resources needed or heads up to make, contact PM

### Future Infrastructure Integration
- Cloud storage and infraestructure (OASIX) - to be defined
- Email service integration - to be defined

## Documentation

### Code Documentation
- Docstrings for all classes and methods

### User Documentation
- Installation guide
- User manual
- Administrator guide
- API documentation

## 8. Contact Information

- Product Manager: [Bernat Pérez]
- Main Developer: [To be assigned]

If you have any doubt or suggestion regarding the project contact the PM.
