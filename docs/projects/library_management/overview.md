# Library Management System — Overview

## Project Goal

In this project, you will build a backend system that manages a library’s collection of items (books, magazines, DVDs, and CDs) and handles user memberships, borrowing, returning, and a promotion system. You’ll apply Object-Oriented Programming (OOP) principles, containerize your application with Docker, and use a database (SQLite or a multi-service Docker Compose setup) to store data.

## Key Features

1. User Management
    * Create users with different membership levels (Student, Basic, Premium).
    * Track their borrowing history.

2. Item Catalog
    * Add various library items (books, magazines, DVDs, CDs).
    * Store each item’s title, type, and availability status.

3. Borrowing & Returning
    * Implement borrowing rules based on membership limits.
    * Enforce due dates and item availability.

4. Promotion System
    * Grant users who have borrowed 15+ books in the last year a double book limit.

## Prerequisites

* Basic Programming Knowledge
You should be comfortable writing APIs in your chosen language (Node.js, Python, Java, etc.).

* Fundamental Database Skills
You’ll need to read and write data to SQLite or another database.

* Familiarity with Docker
You must containerize your app. If you need a DB container (e.g., Postgres), you’ll spin it up with Docker Compose.

## Learning Objectives

1. OOP Principles & API Design

    * Learn how to structure your backend around clear entities (User, Item, Borrowing Records, etc.).
    * Practice building RESTful endpoints for core CRUD operations.

2. Complex Business Rules
    * Enforce membership-based borrowing limits.
    * Track return dates and history.
    * Implement a promotion system based on borrowing frequency.

3. Environment Setup & Deployment
    * Gain experience creating a production-like environment using Docker.
    * Optionally orchestrate multiple services (API, DB) with Docker Compose.

4. Extensibility & Maintenance
    * Understand how modular architecture and clean code practices enable easier feature additions (e.g., new item types, membership tiers).

## What You’ll End Up With

By the end of the project, you’ll have a fully containerized backend that can:

* Create and manage library users with different memberships.
* Store and categorize library items (books, magazines, DVDs, and CDs).
* Handle borrowing and returning with membership limits enforced.
* Apply promotions to frequent borrowers (double book limit).
* Persist data in a database of your choice.

## Next Steps
* Review Your Environment: Make sure Docker is installed, and you’re comfortable running images.
* Check Docker Compose: If you plan to use a separate DB, confirm you can spin up multi-container setups.
* Pick Your Language: If unsure, start with what you know best. Node.js or Python often have simpler boilerplates for junior developers.
* Move on to Step 1: Begin by setting up the core User and Item models, endpoints, and container configuration.