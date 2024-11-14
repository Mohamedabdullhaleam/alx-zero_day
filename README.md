
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


## Overview

The Egyptian National ID Validator API enables validation of Egyptian national ID numbers and extracts key details such as:

- Date of Birth
- Gender
- Governorate of Birth

Using regular expressions, this API ensures the format of each ID is correct and then parses out relevant data. The API is structured to respond with JSON, and Swagger documentation is available for easy testing and exploration.

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

Your server should now be running, and the API is accessible at `http://127.0.0.1:8000`.

## API Endpoints

### Validate National ID
- **URL:** `/api/nid/validate`
- **Method:** POST
- **Description:** Accepts a National ID and verifies its format, extracting the birth date, gender, and governorate if valid.

#### Request Example:
```json
{
  "id": "30005251234567"
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

Interactive API documentation is provided with Swagger UI, allowing you to explore and test the available endpoints.

- **Swagger UI:**
  - **URL:** `/api-docs`
  - **Description:** Allows you to view and test endpoints, providing details on request/response formats.

**Example Documentation URL:** `http://127.0.0.1:8000/api-docs`

## ID Format

The Egyptian National ID is a 14-digit number with the following structure:

| Segment | Description |
| ------- | ----------- |
| **C** (1 digit) | Century indicator (e.g., 2 for 1900–1999, 3 for 2000–2099) |
| **YYMMDD** (6 digits) | Date of birth (YY = year, MM = month, DD = day) |
| **VV** (2 digits) | Governorate code (e.g., 01 for Cairo, 02 for Alexandria) |
| **IIIG** (4 digits) | Birth sequence, where the last digit indicates gender (odd for male, even for female) |
| **X** (1 digit) | Checksum for validation (No clear data or algo found for it) |

### ID Example
**ID:** `30005250234567`

- **Century Code:** `3` (2000–2099)
- **Date of Birth:** `2000-05-25`
- **Governorate:** Alexandria (`02`)
- **Gender:** Female (last digit in sequence is even)

## Error Handling

The API returns structured error responses for invalid inputs or failed validations. Error messages are provided in the response body with appropriate HTTP status codes.

### Error Response Example:

For an invalid National ID:

**Request:**
```json
{
  "id": "30005251234"  // Invalid ID length (only 13 digits)
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
  "id": "29802300101234"  // Invalid birth date: February 30
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

Unit tests are implemented to verify the correctness of the validation logic. They ensure that the API handles all edge cases and follows the National ID format strictly.

### Running Unit Tests:

To run the unit tests using Jest:

1. **Install Jest if not already installed:**
   ```bash
   npm install --save-dev jest
   ```

2. **Run the tests:**
   ```bash
   npm test
   ```

This will run the test suite, checking for edge cases, including valid and invalid IDs.
## Test Cases Covered

The following test cases are covered in the unit tests to ensure the functionality and reliability of the API:

1. **Valid National IDs:**
   - Tests for valid National IDs from various governorates (e.g., Cairo, Alexandria, Beheira).
   - Correct parsing of birth dates, gender, and governorate.
   
2. **Century Code Handling:**
   - Correct handling of century codes (`2` for 1900s and `3` for 2000s) to accurately parse the year of birth.

3. **Date Validation:**
   - **Valid Dates:** Tests for valid dates (e.g., leap years, valid months, valid days).
     - For example, February 29 on leap years like 1996-02-29.
   
   - **Invalid Dates:**
     - **February 30:** February 30 is not a valid date in any year.
     - **April 31:** April has only 30 days, so April 31 is an invalid date.
     - **Invalid Leap Year:** February 29 on non-leap years (e.g., 1999-02-29, which is invalid).
     - **Out of Range Dates:** Dates like `1999-13-32` or `2000-00-00` where the month or day is out of range.

4. **Gender Extraction:**
   - Tests for gender extraction based on the last digit of the birth sequence (odd for male, even for female).
   - Example: ID ending with an odd number indicates male, and even number indicates female.

5. **Governorate Validation:**
   - Tests for valid and invalid governorate codes (mapping the 2-digit governorate code to the corresponding name).
     - For example, governorate code `18` corresponds to "Beheira", and code `01` corresponds to "Cairo".

6. **Invalid IDs:**
   - Tests for invalid IDs (e.g., missing digits, incorrect length, invalid date formats).
     - For example, an ID with only 13 digits instead of 14, or non-numeric characters.

7. **Missing ID and Format Validation:**
   - Tests for cases where the ID is missing or the format is incorrect (less than 14 digits, non-numeric characters).
   - For example, a request without the `id` field, or an ID that is not exactly 14 digits long.

