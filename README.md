
# Egyptian National ID Validator API

A streamlined API for validating Egyptian National IDs and extracting essential information such as date of birth, gender, and birth governorate based on the ID's structure.

## Table of Contents
- [Overview](#overview)
- [Getting Started](#getting-started)
- [API Endpoints](#api-endpoints)
  - [Validate National ID](#validate-national-id)
- [Example Request](#example-request)
- [API Documentation](#api-documentation)
- [ID Format](#id-format)
- [Error Handling](#error-handling)
- [Unit Tests](#unit-tests)
- [Test Cases Covered](#test-cases-covered)
- [Project Flow](#project-flow)

## Overview

The Egyptian National ID Validator API validates Egyptian national ID numbers and extracts key details such as:

- Date of Birth
- Gender
- Governorate of Birth

Using regular expressions, this API ensures the format of each ID is correct and extracts relevant data. It returns responses in JSON format and includes Swagger documentation for easy testing and exploration.

## Getting Started

To set up the project locally, follow these steps:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Mohamedabdullhaleam/NID-Validator.git
   ```

2. **Navigate to the project directory:**
   ```bash
   cd NID-Validator
   ```

3. **Install required dependencies:**
   ```bash
   npm install
   ```

4. **Start the development server (default port: 8000):**
   ```bash
   npm start
   ```

Your server will now be running at `http://127.0.0.1:8000`.

## API Endpoints

### Validate National ID
- **URL:** `/api/nid/validate`
- **Method:** POST
- **Description:** Verifies the format of a National ID and extracts the birth date, gender, and governorate if valid.

#### Request Example:
```json
{
  "id": "30005250234567"
}
```

#### Response Example:
```json
{
  "valid": true,
  "birth_date": "2000-05-25",
  "gender": "Female",
  "governorate": "Alexandria"
}
```

## API Documentation

Interactive API documentation is available with Swagger UI, allowing you to explore and test the available endpoints.

- **Swagger UI:**
  - **URL:** `/api-docs`
  - **Description:** View and test endpoints, providing details on request/response formats.

**Example Documentation URL:** `http://127.0.0.1:8000/api-docs`

## ID Format

The Egyptian National ID consists of 14 digits, structured as follows:

| Segment | Description |
| ------- | ----------- |
| **C** (1 digit) | Century indicator (e.g., 2 for 1900–1999, 3 for 2000–2099) |
| **YYMMDD** (6 digits) | Date of birth (YY = year, MM = month, DD = day) |
| **VV** (2 digits) | Governorate code (e.g., 01 for Cairo, 02 for Alexandria) |
| **IIIG** (4 digits) | Birth sequence, with the last digit indicating gender (odd for male, even for female) |
| **X** (1 digit) | Checksum (no clear algorithm found for validation) |

### ID Example
**ID:** `30005250234567`

- **Century Code:** `3` (2000–2099)
- **Date of Birth:** `2000-05-25`
- **Governorate:** Alexandria (`02`)
- **Gender:** Female (last digit is even)

## Error Handling

The API returns structured error responses for invalid inputs or failed validations.

### Error Response Example:

For an invalid National ID (incorrect length):

**Request:**
```json
{
  "id": "30005251234"
}
```

**Response:**
```json
{
  "valid": false,
  "error": "ID must be exactly 14 digits"
}
```

For an invalid birth date (e.g., February 30):

**Request:**
```json
{
  "id": "29802300101234"
}
```

**Response:**
```json
{
  "valid": false,
  "error": "Invalid birth date"
}
```

## Unit Tests

Unit tests verify the correctness of the validation logic and ensure the API handles edge cases properly.

### Running Unit Tests:

1. **Install Jest if not already installed:**
   ```bash
   npm install --save-dev jest
   ```

2. **Run the tests:**
   ```bash
   npm test
   ```

## Test Cases Covered

The following test cases are covered in the unit tests to ensure functionality and reliability:

1. **Valid National IDs:** 
   - Testing valid IDs across different governorates.
   - Correct extraction of birth date, gender, and governorate.

2. **Century Code Handling:** 
   - Correct interpretation of century codes (`2` for 1900s, `3` for 2000s).

3. **Date Validation:**
   - **Valid Dates:** Tests valid dates, including leap years.
   - **Invalid Dates:** Detects invalid dates such as February 30, April 31, and non-leap year February 29.

4. **Gender Extraction:** 
   - Correct gender extraction based on the last digit (odd for male, even for female).

5. **Governorate Validation:** 
   - Validates known governorate codes and maps them to their corresponding names.

6. **Invalid IDs:** 
   - Tests for invalid IDs (missing digits, incorrect length, invalid date formats).

7. **Missing ID & Format Validation:** 
   - Tests for missing `id` field or incorrect ID format (less than 14 digits, non-numeric characters).

## Project Flow

### 1. **Request Validation**
   - The API validates the ID format to ensure it's exactly 14 digits and numeric.

### 2. **Century Code Validation**
   - The first digit represents the century code (`2` for 1900–1999, `3` for 2000–2099).

### 3. **Birth Date Validation**
   - The birth date (`YYMMDD`) is validated for year, month, and day. Invalid dates return an error.

### 4. **Leap Year Validation**
   - If the birth date is February 29, it is validated for leap year.

### 5. **Governorate Code Validation**
   - The governorate code (`VV`) is checked for validity.

### 6. **Gender Validation**
   - The gender is extracted from the last digit of the birth sequence (odd for male, even for female).

### 7. **Final Validation**
   - If all checks pass, the API returns the extracted birth date, gender, and governorate.

### 8. **Error Handling**
   - Returns `400` errors for missing or invalid IDs with relevant error messages.

### 9. **Swagger Documentation**
   - Swagger UI available at `/api-docs` for easy endpoint testing.

### 10. **Edge Cases & Tests**
   - Handles edge cases like invalid dates, leap years, and gender extraction based on the birth sequence.
