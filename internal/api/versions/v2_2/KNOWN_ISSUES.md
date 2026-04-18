# Known Issues — v2.2 API Implementation

Identified during systematic audit against `Full_api.json` on April 17, 2026.
These are deferred bugs not addressed in the naming/comment pass. Each issue
is labelled with a severity: **Critical**, **High**, or **Medium**.

---

## Enum Value Mismatches

### KI-001 — `LabelTypeEnum` — All values wrong [Critical]
**File:** `models.go`

Go constants do not match the spec. Every value is shifted and the `All` variant is absent.

| Go Constant | Go Value | Spec Value | Spec Name |
|---|---|---|---|
| `LabelTypeEnum_Project` | 0 | 2 | `Project` |
| `LabelTypeEnum_Finding` | 1 | 0 | `Finding` |
| `LabelTypeEnum_Asset` | 2 | 3 | `Assets` |
| `LabelTypeEnum_Client` | 3 | 1 | `Client` |
| *(missing)* | — | 4 | `All` |

**Fix:** Reassign all constants to match the spec and add `LabelTypeEnum_All = 4`.

---

### KI-002 — `ApiRolesEnum` — All Pentester values off-by-one; two values missing [Critical]
**File:** `models.go`

The spec has 12 values (0–11). Go has 10 (0–9). The spec `Client = 4` is entirely absent,
causing every Pentester role to be assigned an integer one lower than intended.

| Go Constant | Go Value | Spec Value | Spec Name |
|---|---|---|---|
| `ApiRolesEnum_Client` | 3 | 3 | `Client_General` (rename needed) |
| *(missing)* | — | 4 | `Client` |
| `ApiRolesEnum_Pentester_View_Only` | 4 | 5 | `Pentester_View_Only` |
| `ApiRolesEnum_Pentester_Project_Only` | 5 | 6 | `Pentester_Project_Only` |
| `ApiRolesEnum_Pentester_General` | 6 | 7 | `Pentester_General` |
| `ApiRolesEnum_Pentester_ProjectManager` | 7 | 8 | `Pentester_ProjectManager` |
| `ApiRolesEnum_Pentester_Manager` | 8 | 9 | `Pentester_Manager` |
| `ApiRolesEnum_Pentester_Owner` | 9 | 10 | `Pentester_Owner` |
| *(missing)* | — | 11 | `Pentester_Team_Manager` |

**Fix:** Add `ApiRolesEnum_Client = 4`, shift all Pentester constants up by 1, add `ApiRolesEnum_Pentester_Team_Manager = 11`.

---

### KI-003 — `FormFieldTypeEnum` — Entirely wrong values and names [Critical]
**File:** `models.go`

Go starts at 0; spec starts at 1. Names are completely different.

| Go Constant | Go Value | Spec Value | Spec Name |
|---|---|---|---|
| `FormFieldTypeEnum_Text` | 0 | 1 | `Text` |
| `FormFieldTypeEnum_Number` | 1 | 2 | `Multitext` |
| `FormFieldTypeEnum_Date` | 2 | 3 | `Dropdown` |
| `FormFieldTypeEnum_Select` | 3 | 4 | `Multiselect` |
| `FormFieldTypeEnum_TextArea` | 4 | *(not in spec)* | — |

**Fix:** Remove all five constants, replace with `Text=1`, `Multitext=2`, `Dropdown=3`, `Multiselect=4`.

---

### KI-004 — `ContinuousProjectVulnerabilityTypeEnum` — Missing value 3 [High]
**File:** `models.go`

`TenableWebAppScan = 3` is defined in the spec but absent from Go.

**Fix:** Add `ContinuousProjectVulnerabilityTypeEnum_TenableWebAppScan = 3`.

---

### KI-005 — `ImportFileTypeEnum` — Missing value 27 [Medium]
**File:** `models.go`

`Horizon = 27` is defined in the spec but absent from Go.

**Fix:** Add `ImportFileTypeEnum_Horizon = 27`.

---

## Struct Field Mismatches

### KI-006 — `CreateOrUpdateFindingRequest` — Severely incomplete [Critical]
**File:** `models.go`

The struct has only 5 fields. The spec defines 26. Missing fields:

`code`, `type` *(required)*, `complianceStatus`, `complianceComment`, `impact`,
`impactDescription`, `likelihood`, `likelihoodDescription`, `recommendation`,
`backgroundInformation`, `cvss`, `projectTaskId`, `reviewerId`, `cweList`,
`cveList`, `mitreAttackTacticsList`, `mitreAttackTechniquesList`,
`mitreAttackMitigationsList`, `vulnerabilityTypeList`, `externalUrlList`,
`assetIdList`, `labelIdList`, `projectControlIdList`, `findingEvidenceList`,
`customFields`.

Also: `Severity` uses `*FindingSeverityEnum` but the spec requires `FindingCriticalityEnum`.
Also: `ProjectID` (`projectId`) is a request body field in Go but is a **query parameter** in the spec.

---

### KI-007 — `CreateClientRequest` — Wrong shape [Critical]
**File:** `models.go`

Go struct has fields that do not exist in the spec, and is missing all spec-defined optional fields.

| Status | Field | Notes |
|---|---|---|
| Missing | `status` | Required by spec (`ClientStatusEnum`) |
| Missing | `clientNumber` | nullable string |
| Missing | `accountManagerId` | nullable uuid |
| Missing | `labelIdList` | array of uuid |
| Missing | `teamIdList` | array of uuid |
| Missing | `clientInformation` | nullable object |
| Extra (not in spec) | `Description` | — |
| Extra (not in spec) | `Email` | — |
| Extra (not in spec) | `Phone` | — |
| Extra (not in spec) | `Address` | — |

---

### KI-008 — `CreateProjectRequestV2` — Missing fields and wrong field name [High]
**File:** `models.go`

| Status | Field | Notes |
|---|---|---|
| Missing | `code` | nullable string |
| Missing | `status` | nullable string |
| Missing | `projectTemplateId` | nullable uuid |
| Wrong name | `labelIds` → should be `labelIdList` | json tag mismatch |
| Extra (not in spec) | `Description` | — |

---

### KI-009 — `AssetDto` and `CreateOrUpdateAssetRequest` — Wrong JSON tag and missing fields [High]
**File:** `models.go`

`OS *string json:"os"` — spec field name is `operatingSystem`. The JSON tag `"os"` will
serialize/deserialize incorrectly.

Both structs are also missing: `documentationLink`, `identifier`, `category`, `group`.
`CreateOrUpdateAssetRequest` additionally missing: `testingFrequency`, `labelIdList`.

---

### KI-010 — `CreateOrUpdateFindingEvidenceRequest` — Missing `assetId` field [High]
**File:** `models.go`

The spec includes `assetId` (nullable uuid) in `CreateOrUpdateFindingEvidenceRequest`.
The Go struct does not have this field.

**Fix:** Add `AssetId *string json:"assetId,omitempty"`.

---

### KI-011 — `PlanningDateDtoV2` — Entirely wrong fields [High]
**File:** `models.go`

Go has `Date` and `Description`. Spec has `status` (`PlanningDateStatusEnum`), `startDate`, `endDate`.

---

### KI-012 — `TaskGroupTemplateDto` and `TaskTemplateDto` — Missing fields [Medium]
**File:** `models.go`

`TaskGroupTemplateDto` missing: `code` (nullable string).
`TaskTemplateDto` missing: `code` (nullable string), `externalUrl` (nullable string).

---

### KI-013 — `RefreshTokenResult` — Extra fields not in spec [Medium]
**File:** `models.go`

`RefreshToken` and `TokenType` are present in Go but do not exist in the spec schema.
They will never be populated by the API. Safe to remove.

---

### KI-014 — `FindingDtoPagedResultDtoAjaxResponse.TargetUrl` — Missing `omitempty` [Medium]
**File:** `models.go`

`TargetUrl *string json:"targetUrl"` — every other AjaxResponse wrapper uses `omitempty`.
This will serialize `"targetUrl": null` in JSON output instead of omitting the field.

---

## Missing Structs

### KI-015 — `FormDataDto` — Type name used in spec but not defined in Go [Medium]
**File:** `models.go`

`RequestProjectFormDto.formData` references `FormDataDto` in the spec.
Go uses `RequestFormDataDto` instead (a type that does not appear in the spec).
If both types have the same field shapes this may work at runtime, but the name is wrong.

---

### KI-016 — `ClientInformationDto` — Referenced in spec but not defined [Medium]
**File:** `models.go`

`CreateClientRequest.clientInformation` references `ClientInformationDto`. No such struct exists.
Blocked by KI-007 (the parent struct is itself wrong).

---

## Functional Bugs in ops files

### KI-017 — `GetProjectByID` (now `ApiV22ClientProjectsByIdGet`) — Nil response pointer [Critical]
**File:** `client_ops.go` (fixed in renaming pass — verify fix is complete)

Original code passed `nil` as the response pointer to `DoRequest`, then declared the
response variable *after* the call. The function always returned a zero-value struct.
**Status:** Partially addressed during rename rewrite — confirm `DoRequest` now receives `&response`.

---

### KI-018 — `ApiV22PentesterFindingEvidencePost` — Uses non-spec legacy path [High]
**File:** `pentester_ops.go`

Uses path `/api/services/app/Finding/CreateOrEditFindingInstance` which does not exist in
`Full_api.json`. The correct spec path for creating evidence is
`POST /api/v2.2/pentester/findings/{findingId}/evidences`
(implemented separately as `ApiV22PentesterFindingsByFindingIdEvidencesPost`).
This function is a legacy internal service call and should be deprecated or removed.

---

### KI-019 — Token auth paths missing version prefix [High]
**File:** `token_auth_ops.go`

`ApiTokenauthAuthenticatePost`, `GetUserId`, `ApiTokenauthRefreshtokenPost`, and
`ApiTokenauthSendtwofactorauthcodePost` all use bare `/api/TokenAuth/...` paths.
All other ops files use `/api/v2.2/` as the base. Confirm whether the token auth
endpoints intentionally bypass versioning (some APIs do this for auth routes).

---

### KI-020 — Missing functions: GET evidences list, UPDATE/DELETE client user [High]
**File:** `pentester_ops.go`, `client_ops.go`

The following spec endpoints have no corresponding Go function:
- `GET /api/v2.2/pentester/findings/{id}/evidences` → `ApiV22PentesterFindingsByIdEvidencesGet`
- `PUT /api/v2.2/client/users/{id}` → `ApiV22ClientUsersByIdPut`
- `DELETE /api/v2.2/client/users/{id}` → `ApiV22ClientUsersByIdDelete`

Also: `message_ops.go` contains only a package declaration — no functions implemented.

---

*End of known issues.*
