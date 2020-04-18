sequenceDiagram 
    participant A as Alice
    participant B as Bill
    participant NT as NextTrace
    participant DS as DataStore
    participant PH as Public Health
    A->>PH: Alice gets a test
    PH->>A: Positive COVID result
    PH->>NT: Alice's contact info shared
    NT->>DS: Survey code created for Alice
    NT->>A: Survey invite sent with code
    alt rejects Survey
        A->>NT: rejects invite
        NT->>DS:Alice's info deleted
    else accepts Survey
        A->>NT: Fills Survey. Provides contacts
        NT->>DS: Validates code
        alt code valid
            NT->>DS: stores result
            NT->>B: Notifies Bill. Recommends isolation
            NT->>PH: Sends result via webhook
            PH->>B: Arranges testing
            alt Bill is positive
                PH->>NT: NT is notified of Bill's result
                NT->>B: Sends Bill survey invite with code
            else Bill is negative
                NT->>B: All clear
            end
        else code invalid
            NT->>DS: Deletes result
        end
    end
