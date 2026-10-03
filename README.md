# TareaPOOBanking


Se hizo herencia de una clase identidad abstracta a los diferentes tipos de identidades, esta clase identidad abstracta forma parte de una lista en usuario
Todos los sistemas bancarios implementan una interfaz llamada BankProcessor que tiene una funcion process que pueden usar segun se requiera

classDiagram

class BankingService {
    +execute(Identity, BankOperation, String): OperationResult
    +isOperationAllowed(IdentityType, OperationType): boolean
    +checkIdentityRules(Identity, BankOperation, String): String
    +dailyLimit(Identity): double
    +isProcessorAllowed(Identity, String): boolean
    +processorSupports(OperationType): boolean
    +calculateFee(String, BankOperation, double): double
    +dispatch(Identity, BankOperation, double): String
}

class BankProcessor {
    <<interface>>
    +process(String, String, double, String): String
}

class NationalBankProcessor {
    +postTransaction(String, String, double, String): String
}

class PacificBankProcessor {
    +submit(String, String, double, String): String
    +submitPayroll(String, List~String~, String): String
}

class SwiftGatewayProcessor {
    +sendWire(String, String, double, String): String
}

class BankOperation {
    -operationType: OperationType
    -amount: double
    -destinationAccount: String
    -destinationIBAN: String
    -currency: String
    -sourceAccount: String
    -payrollAccounts: List~String~
}

class User {
    -id: String
    -fullName: String
    -identities: List~Identity~
    +addIdentity(Identity)
    +getIdentities(): List~Identity~
}

class Identity {
    #type: IdentityType
    #documentNumber: String
    #accountNumber: String
    #accountName: String
    #dailyLimit: double
    +getType(): IdentityType
    +getDailyLimit(): double
}

class PersonalIdentity {
    #dailyLimit
    +getDailyLimit(): 
}

class BusinessIdentity {
   -String companyName
    #dailyLimit
    +getDailyLimit(): 
}

class MinorIdentity {
    #guardianName: String
    #dailyLimit
    +getDailyLimit(): 
}

class ForeignResidentIdentity {
    #countryCode: String
    #residencyExpiresOn: LocalDate
    #dailyLimit
    +getDailyLimit(): 
}

class OperationResult {
}

class DailyUsageTracker {
}

class AuditLog {
}

class OperationType {
    <<enumeration>>
    DEPOSIT
    WITHDRAWAL
    DOMESTIC_TRANSFER
    INTERNATIONAL_TRANSFER
    PAYROLL
}

class IdentityType {
    <<enumeration>>
    PERSONAL
    BUSINESS
    MINOR
    FOREIGN_RESIDENT
}

BankProcessor <|.. NationalBankProcessor
BankProcessor <|.. PacificBankProcessor
BankProcessor <|.. SwiftGatewayProcessor

Identity <|-- PersonalIdentity
Identity <|-- BusinessIdentity
Identity <|-- MinorIdentity
Identity <|-- ForeignResidentIdentity

User "1" *-- "0..*" Identity

BankingService --> BankProcessor
BankingService --> User
BankingService --> BankOperation
BankingService --> OperationResult
BankingService --> DailyUsageTracker
BankingService --> AuditLog

BankOperation --> OperationType
Identity --> IdentityType
