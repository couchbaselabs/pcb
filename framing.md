## Framing

Request, Response, and Stream Message bodies are protobuf encoded.

### Message Types

Type is a single byte (u8)

    REQUEST = 1:
        RqID(u16) BodyLen(u32) Body
        // Paired with a response
    
    RESPONSE = 2:
        RqID(u16) BodyLen(u32) Body

        // The stream IDs started by a response are included here so
        // that a relaying / proxy layer can pair streams to clients.
    
    STREAM_BEGIN = 3:
        StreamID(u16) RqId(u16)

        // All streams MUST send their STREAM_BEGIN before the
        // associated RESPONSE is sent

    STREAM_MESSAGE = 4:
        StreamID(u16) BodyLen(u32) Body
    
    STREAM_CLOSE = 5:
        StreamID(u16)
    
    REQUEST_ERRONLY = 6:
        RqID(u16) BodyLen(u32) Body

        // Only response will be an error

        // Perhaps require that RqID is smaller than
        // some size so proxies can do non-tabling
        // client-request pairing for these, since
        // they may not have responses and can't
        // be cleaned out.

