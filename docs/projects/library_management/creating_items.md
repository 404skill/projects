# Creating Items

## Overview  
   In this second section of the Library Management System, you’ll focus on defining and managing items that the library offers for borrowing. At this stage, the core item types are:
   - Books  
   - Magazines  
   - DVDs  

   Later, you’ll add **CDs** (and potentially more) to the system. For now, make sure you can create, store, and retrieve these items in your chosen database or data structure.

## Data Model  
   - **item_id**  
     A unique identifier for each item (UUID, auto-increment integer, etc.).  
   - **name**  
     The title or name of the item (e.g., “Harry Potter and the Chamber of Secrets,” “National Geographic,” “The Lion King” DVD).  
   - **type**  
     One of ["book", "magazine", "dvd"] at this point. (You’ll add "cd" later.)  
   - **status** (optional at this stage, but recommended)  
     Indicates if an item is "available" or "borrowed." This will be critical once you implement borrowing and returning logic.

## Database Options
   Similar to user storage, you can use:
   - A SQL database table, for example:
     ```
     CREATE TABLE items (
       item_id TEXT PRIMARY KEY,
       name TEXT NOT NULL,
       type TEXT NOT NULL,
       status TEXT DEFAULT 'available'
     );
     ```
   - SQLite or an in-memory approach if you want something simpler.  
   - A JSON file or in-memory array for quick prototyping.  

   Whichever method you choose, the structure should support at least the four fields listed above.

## Endpoints  

   a. **POST /items**  
   - **Purpose**: Create a new item record.  
   - **Request Body** example:
     ```
     {
       "name": "National Geographic - June Edition",
       "type": "magazine"
     }
     ```
   - **Implementation**:  
     1. Validate that `name` and `type` exist.  
     2. Ensure `type` is one of ["book", "magazine", "dvd"].  
     3. Generate a unique `item_id`.  
     4. Insert the item into your items table (SQL) or data store.  
   - **Response** example:
     ```
     {
       "item_id": "123e4567-e89b-12d3-a456-426614174020",
       "name": "National Geographic - June Edition",
       "type": "magazine",
       "status": "available"
     }
     ```
   - **Status Codes**:  
     - 201 Created if successful.  
     - 400 Bad Request if validation fails.  

   b. **GET /items** (optional but helpful)  
   - **Purpose**: Retrieve a list of all items.  
   - **Implementation**: Return an array of item objects from your data store.  
   - **Response** example:
     ```
     [
       {
         "item_id": "abc123",
         "name": "Harry Potter and the Chamber of Secrets",
         "type": "book",
         "status": "available"
       },
       {
         "item_id": "def456",
         "name": "Madagascar",
         "type": "dvd",
         "status": "available"
       }
     ]
     ```

   c. **GET /items/:id** (optional)  
   - **Purpose**: Retrieve a single item by its ID.  
   - **Response**: The item object, or a 404 if not found.

## Data Validation  
   - Make sure you only accept valid types ("book", "magazine", "dvd").  
   - You can extend your validation (for example, ensuring `name` is not empty).  

## Testing Your Item Endpoints  
   - Use Postman, curl, or a similar tool to create items via `POST /items`.  
   - Verify you can retrieve them with `GET /items` and optionally `GET /items/:id`.  
   - Check that your application handles error cases (missing name, invalid type, etc.) gracefully.

## Next Steps  
   After setting up item creation and retrieval, you’ll be ready to tackle borrowing logic, which brings together Users and Items. You’ll enforce membership-based borrowing limits and track which items are out on loan vs. available. That’s where the `status` field on each item will become essential.  

   Continue to the next section where you’ll learn how to implement and manage the borrowing/returning functionality.