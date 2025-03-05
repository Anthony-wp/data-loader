# Data Loader

Data Loader is a modular Java project designed to fetch and synchronize security-related data (such as CVE, CPE, and other vendor-specific datasets) from various external APIs and store the results in MongoDB. The project currently includes a complete implementation for the [NVD](https://nvd.nist.gov/) vendor and provides a robust abstraction layer to easily extend support for new vendors.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Running the Project](#running-the-project)

## Overview

The goal of this project is to automatically request and retrieve security data (e.g., CVE and CPE) from various vendors and store it in MongoDB for further analysis. With an existing implementation for the NVD vendor, the project's architecture is designed to be highly extensible so that additional vendor integrations can be added with minimal effort.

## Architecture

The project is structured around two primary components:

1. **Data Loader**  
   - **Purpose:** Fetch vendor data using pagination and batch processing.
   - **Implementation:**  
     - Uses the `AbstractDataLoader` class for full and incremental synchronization.
     - Handles tasks asynchronously and supports resuming incomplete tasks via `TaskInstanceService`.

2. **Data Sync**  
   - **Purpose:** Synchronize vendor-specific documents with global representations in MongoDB.
   - **Implementation:**  
     - Uses the `AbstractDataSync` class alongside the `GlobalDataSynchronizer` interface.
     - Compares timestamps and updates or inserts new data accordingly.
     
Additional services include a scheduler for running tasks based on cron expressions and REST controllers for manual task triggering.

## Features

- **Vendor Agnostic Design:**  
  Easily extendable architecture for adding new vendors.

- **Asynchronous Processing:**  
  Utilizes asynchronous task execution (`@Async`) and retry mechanisms (`@Retryable`) for robust data fetching.

- **Batch Processing:**  
  Supports paginated API calls and bulk database operations for efficient data handling.

- **Task Management:**  
  Tracks task instances, supports task resumption, and manages scheduled jobs through integrated services.

## Prerequisites

Before running the project, ensure you have the following installed:

- **Java 11 or higher**
- **Maven** (for building the project)
- **Docker**  
  The project requires Docker to run a MongoDB instance. Make sure Docker is installed and running on your system.

## Installation and Setup

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/Anthony-wp/data-loader.git
   cd data-loader
   ```
2. **Configure MongoDB:**

  You can run a MongoDB instance using Docker. For example:
  
  ```bash
  docker run --name mongodb -p 27017:27017 -d mongo
  ```
  Alternatively, if you have a `docker-compose.yml` file, you can start the services with:
  
  ```bash
  docker-compose up -d
  ```
3. **Build the Project:**

  Use Maven to build the project:
  ```bash
  mvn clean install
  ```

## Running the Project
Once you have your environment set up and MongoDB is running:

1. **Start the Application:**

  You can run the application via your IDE (e.g., IntelliJ IDEA) or from the command line:

  bash
  ```
  mvn spring-boot:run
  ```
2. **Accessing the Endpoints:**

  The project includes REST endpoints (e.g., `/tasks/start`, `/tasks/stop`, `/healthCheck/check`) to manage and monitor data loader tasks. Use tools like Postman or your browser to test these endpoints.
