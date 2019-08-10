# Documents for MyHealthData

## [FHIR](http://hl7.org/fhir/) [resources](https://www.hl7.org/fhir/resourcelist.html)

기본적으로 모든 환자 관련 정보는 [FHIR](http://hl7.org/fhir/) [resources](https://www.hl7.org/fhir/resourcelist.html) 형태로 저장

Why?
- 어차피 내부적으로 database scheme를 짜야하는데 이걸 맘대로 만들기 보다는, 이미 많이 고민한 프로젝트에서 정의한 scheme를 따라가는게 맞다고 생각함
- 나중에 외부에서 import하고 또 외부로 export하는 과정에서 별도의 데이터 변환 없이 바로 활용할 수 있다는 장점이 있음

Which?
- [Patient resource](https://www.hl7.org/fhir/patient.html)
  - 환자의 기본적인 demographic information을 저장하기 위한 목적
- [Observation resource](https://www.hl7.org/fhir/observation.html)
  - 개별 환자의 여러 측정 값을 저장하기 위한 목적

### Patient resource https://www.hl7.org/fhir/patient.html

환자의 demographic information 관련된 모든 정보는 이 형식으로 저장

사용 데이터 (key) 목록
- id
- identifier
- name: HumanName
- gender: code
- birthDate: date
- text


Json Example
```json
  {
    "id": "medibloc-mydata-v1-20190806-XSEWER24-1-SDFW4EWR" 
      //medibloc-mydata-v1 version에 따라 바르겠지만, 일반 표지자 + 날짜 + 랜덤숫자문자(based on 기기번호) + 순번 + 랜덤숫자문자(based on 환자번호)
    "resourceType": "Patient",
    "name": [             // 이름은 여러가지가 가능합니다.
      {
        "use": "usual", // usual | official | temp | nickname | anonymous | old | maiden
        "text": "이은솔" // 성과 이름 붙여서 쓸려다보니... 좀 더 코드화 안된 느낌이지만 이걸 쓰기로
      }
    ],
    "gender": "male", // male | female | other | unknown
    "birthDate": "1985-02-14" // 1985 1985-02 1985-02-14  같은 형태 가능
  }
```

### Observation resource https://www.hl7.org/fhir/observation.html

환자의 vital signs (키 몸무게 포함), 검사 결과, 임상적인 특징, 흡연 여부 등, ... 모두 여기에 속함

사용 데이터 목록
- id
- status: code (필수임)
- code: CodeableConcept
- effectiveDateTime ? Timing?
- issued ? 
- value ?


Json Example (using LOINC)
```json
  {
    "id": "medibloc-mydata-v1-20190809-XSEWER24-1-SDFW4EWF",
    "resourceType": "Observation",
    "status": "final",
    "code": {
      "coding": [
        {
          "system": "http://loinc.org",
          "code": "8302-2",
          "display": "Body height"
        },
        {
          "system": "http://loinc.org",
          "code": "85353-1",
          "display": "Vital Signs"
        }
      ]
    },
    "subject": {
      "reference": "Patient/001",
      "display": "이은솔" 
    },
    "effectiveDateTime": "1985-02-14",
    "valueQantity": {
      "value": 7.2,
      "unit": "g/dl",
      "system": "http://unitsofmeasure.org",
      "code": "g/dL"
    },
    "interpretation": [
    {
      "coding": [
        {
          "system": "http://terminology.hl7.org/CodeSystem/v3-ObservationInterpretation",
          "code": "L",
          "display": "Low"
        }
      ]
    }
  ],
  "referenceRange": [
    {
      "low": {
        "value": 7.5,
        "unit": "g/dl",
        "system": "http://unitsofmeasure.org",
        "code": "g/dL"
      },
      "high": {
        "value": 10,
        "unit": "g/dl",
        "system": "http://unitsofmeasure.org",
        "code": "g/dL"
      }
    }
  ]
  },
```




### Patient

## [LOINC](https://loinc.org/) code for life log data

### Vital Signs

Vital Signs: 85353-1 https://s.details.loinc.org/LOINC/85353-1.html?sections=Comprehensive
- It was created for HL7 FHIR Vital Signs Profile
- It includes body height, body weight, Body mass index (BMI) [Ratio]	

Body height: 8302-2 https://s.details.loinc.org/LOINC/8302-2.html?sections=Comprehensive + (Vital Signs: 85353-1)

Body weight: 29463-7 https://r.details.loinc.org/LOINC/29463-7.html?sections=Comprehensive + (Vital Signs: 85353-1)

Body mass index (BMI): 
39156-5 https://r.details.loinc.org/LOINC/39156-5.html?sections=Comprehensive + (Vital Signs: 85353-1)

Blood pressure panel with all children optional: 85354-9 https://r.details.loinc.org/LOINC/85354-9.html?sections=Comprehensive
- It includes Systolic blood pressure, Diastolic blood pressure

Systolic blood pressure: 8480-6 https://r.details.loinc.org/LOINC/8480-6.html?sections=Comprehensive + (Blood pressure panel with all children optional: 85354-9) + (Vital Signs: 85353-1)

Diastolic blood pressure: 8462-4 https://r.details.loinc.org/LOINC/8462-4.html?sections=Comprehensive + (Blood pressure panel with all children optional: 85354-9) + (Vital Signs: 85353-1)

### Others

Adult Waist Circumference Protocol: 56086-2 https://s.details.loinc.org/LOINC/56086-2.html?sections=Comprehensive (It could be changed)

Pedometer tracking panel: 55413-9 https://r.details.loinc.org/LOINC/55413-9.html

Number of steps in unspecified time Pedometer: 55423-8 https://s.details.loinc.org/LOINC/55423-8.html?sections=Comprehensive

Distance walked in unspecified time Pedometer: 55430-3 https://r.details.loinc.org/LOINC/55430-3.html?sections=Comprehensive