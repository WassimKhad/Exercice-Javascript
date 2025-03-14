    DataInsightResponse:
      type: object
      required:
        - graphs
      properties:
        graphs:
          type: array
          description: >-
           A list  of graphs about a particular set of channels, representing the merchant data aggregated over a certain period (year, or month)
          
          items:
            $ref: '#/components/schemas/Graphs'
          minItems: 0
    Graphs:
      type: object
      description: >-
        Characteristics of graphs: data and types
      required:
        - year
        - type
        - channels
        
      properties:
        type:
          $ref: '#/components/schemas/GraphIdDataInsightEnum'
        year:
          type: string
          description: >-
            The year concerning the data
          example: 2023 
        monthNumber:
          type: string
          description: >-
            The month in the year concerned with the data.
          example: "02"
        channels:
          type: array
          description: list of Channels
          items:
            $ref: '#/components/schemas/ListOfChannels'
          minItems: 1
    ListOfChannels:
      type: object
      required:
        - type
        - items
      properties:
        type:
          $ref: '#/components/schemas/ChanelDataInsightEnum'
        clientAverage:
          type: number
          format: float
          description: >
           - Average of the figures in itemId for the client. 
           
           - If the average is not applicable for this type of graph, the value will be null.
          example: 20.23
        
        sectorAverage:
          type: number
          format: float
          description: >-
           - Average of the figures in itemId for the sector.
           
           - If the average is not applicable for this type of graph, the value will be null.
          example: 20.23
        
        items:
          type: array
          description: list of items
          items:
            $ref: '#/components/schemas/ListOfItemsDataInsight'
          minItems: 1          
    ListOfItemsDataInsight:
      type: object
      description: A graph component
      required:
        - id
        - clientValue
        - unit
      properties:
        id:
          type: string
          description: Id of the item. 
          example: MAN      
        graphOrder:
          type: integer
          format: int32
          description: Position of the item in the graph. 
          example: 1 
        clientValue:
          type: number
          format: float
          description: Value of the figures in itemId for the client. 
          example: 20.23 
        sectorValue:
          type: number
          format: float
          description: Value of the figures in itemId for the sector. NULL for non significative data 
          example: 20.23
        unit:
          $ref: '#/components/schemas/UnitDataInsightEnum'
        clientRange:
          $ref: '#/components/schemas/RangeDataInsightEnum'
        sectorRange:
          $ref: '#/components/schemas/RangeDataInsightEnum'
        
    GraphIdDataInsightEnum:
      type: string
      description: Type of the graph.
      enum:
        - SEX_REP
        - AGE_REP
        - MAP_REP
        - BUS_REP
        - SPC_REP
        - AGE_BAS
        - MAP_BAS
        - SEX_BAS
        - BUS_BAS
        - SPC_BAS
        - CBVISA_REP
        - PAYMETH_REP
        - WALLET_REP
        - REIMBST_REP
    ChanelDataInsightEnum:
      type: string
      description: >-
        - Channel type (E-commerce, Shop, Omnicanal).
        
         - SHOP: Shop
         - OMNI: Omnicanal
         - E_COM: E-commerce
      enum:
        - SHOP
        - OMNI
        - E_COM        
    UnitDataInsightEnum:
      type: string
      description: Unit of the figures in itemValueClient and itemValueSector
      enum:
        - PERCENT
        - EURO
        - YEAR
    RangeDataInsightEnum:
      type: string
      description: This field is usefull in the MAP-BAS graphs to classify the items into 6 ranges; and enable the FRONT to display the items in the right coulor on the map.
      enum:
        - MAX_90
        - BETWEEN_85_90
        - BETWEEN_85_90
        - BETWEEN_80_85
        - BETWEEN_75_80
        - MIN_75  
    
