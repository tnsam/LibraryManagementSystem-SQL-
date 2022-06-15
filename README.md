# LibraryManagementSystem-SQL-

## Background of the system
The library management system is a system that allows staff or librarians to perform daily operations. This system consists of four functions which are borrower transaction management, book transaction management, discussion room transaction management, and staff management. This system will be operated by TAR UC library staff.

The borrower transaction management function allows the librarian to perform transactions including loan of books, returning of books and generation of useful reports for analysis purposes. It also can be used by the librarian to monitor the borrowers’ transactions by retrieving the borrowers’ records stored in the database by using borrowers’ ID or borrowers’ name. Thus, the librarian is able to check if the borrower has any book late returned or book lost. 

### Assumptions and Business Rules
#### Borrower
1. One borrower can have many transactions, but each transaction belongs to one borrower only. (One-to-many)
2. For student type borrowers, the maximum number of books can be borrowed is 3.
3. For student type borrowers, the maximum borrowing period is 14 days.
4. For lecturer type borrowers, the maximum number of books can be borrowed is 5.
5. For lecturer type borrowers, the maximum borrowing period is 21 days.
6. The number of books lost for each borrower cannot be more than or equal to 3.

#### Borrower transaction
1. One transaction namely borrow or return book only belongs to one borrower. (One-to-one)
2. One transaction namely borrow or return book only belongs to one book. (One-to-one)
3. One transaction namely borrow or return book only belongs to one staff member. (One-to-one)
4. The formula for calculating fines payment for late return of book: 
    lateReturnFines = noOfDaysLateReturn * 0.5
5. The formula for calculating fines payment for book lost:
    bookLostFines = priceOfBook * 2
6. The borrower must pay for the late return fines once they return the book.
7. The borrower must pay for the book lost fines once they notify the staff the book is lost.

### Queries, Procedures, Triggers and Reports 
#### Query 1: Operational query - Borrower Transaction Details
Purpose: The purpose of this query is to allow the librarians to trace the borrowers who did not return the book after the due date of borrowing period for more than 14 days, so the librarian can email to notify the borrower to return the book as soon as possible. 

#### Query 2: Tactical query - Borrower Transaction Analysis
Purpose: The purpose of this query is to retrieve borrowers who have exceeded the maximum number of books lost. This allows the management to identify which borrower has exceeded the maximum number of books lost and the management can take some action such as blacklist these borrowers from borrowing books. 

#### Query 3: Strategic query - Comparison of number of borrower transactions for each month in a year
Purpose: The purpose of this query is to check which borrower type is more likely to return a book on time, late return or book lost. For example, the management can compare the number of borrower transactions for ‘Late Return’ status and then modify the borrowing period for that borrower type so that the borrower is less likely to return the book after the due date of the borrowing period. 

#### Procedure 1: Borrow book
Purpose: The purpose of this procedure is to allow the borrowers to borrow books. For student type borrowers, they can borrow maximum 3 books while lecturer type borrowers can borrow maximum 5 books. The maximum borrowing period for the student type borrowers is 14 days while the maximum borrowing period for the lecturer type borrowers is 21 days. After the borrower borrows a book, the number of books on hand for the borrower and quantity of the book will be updated. 

#### Procedure 2: Return book
Purpose: The purpose of this procedure is to allow the borrowers to return books. After the borrower returns a book, the number of books on hand for the borrower and quantity of the book will be updated. If the borrower returns the book after the due date of the borrowing period, then the fine amount will be calculated and the borrower needs to pay for the fines immediately. On the other hand, if the book is lost, then the borrower needs to pay for the fines which is double the amount of the book price. 

#### Trigger 1: Validate if the borrowers is valid for borrowing books
Purpose: The purpose of this trigger is to ensure that the borrowers did not exceed the maximum book borrowing and book lost limit before the borrower is allowed to borrow a book. For example, the borrowers cannot have a number of books lost more than or equal to 3, if the borrower has more than or equal to 3 number of books lost, then the borrower is not allowed to borrow the book. Moreover, for student borrower types, the number of books on hand cannot be more than 3 while for lecturer borrower types, the number of books on hand cannot be more than 5. If their number of books on hand exceeds the maximum amount, then they are not allowed to borrow books. 

#### Trigger 2: Validate if the borrower transaction is duplicated
Purpose: The purpose of this trigger is to check if the borrowed transaction with the same borrower and book is duplicated and prevent the duplicate transaction from being inserted. This is because the borrower might not borrow two books with the same ISBN at the same time. Thus, once the transaction with the same borrower and book is inserted two times at the same time, then the second transaction will be counted as duplicated and cannot be inserted. 

#### Trigger 3: Monitor the modification of the borrower transaction details
Purpose: The purpose of this trigger is to trace the modification of the fines amount and transaction status after each borrower has returned the book.

#### Trigger 4: Monitor the modification of the borrowers details
Purpose: The purpose of this trigger is to trace the modification of the noOfBooksOnHand, noOfBookLost and noOfLateReturn for each borrower after borrowing or returning the book.

#### Report 1: Detail report of borrower’s transactions 
Purpose: The purpose of this report is to display the details of borrower transactions for a borrower and the borrower’s favourite book. Also, the total number of transactions, late return, book lost and fines amount for the borrower. Hence, by reading this report, the librarian is able to check the transactions done by the borrower and analyse the borrower’s favourite book. By analysing the favourite book for each borrower, the library management is able to know the most preferred book for different types of borrower namely lecturer and student.

#### Report 2: Summary report of  top 5 most frequent borrowers
Purpose: The purpose of this report is to display the top 5 most frequent borrowers that borrowed a lot of books and return books on time. So, the library management can give them awards in order to encourage them to read more.

#### Report 3: On demand report of borrower’s late return transaction record
Purpose: The purpose of this report is to allow the librarian to analyse which borrower has higher chance for late returning, so the librarian can make decisions based on the late return percentage of each borrower in order to decide whether to blacklist the borrower from borrowing books.
