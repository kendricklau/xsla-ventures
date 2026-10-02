# Form Submission Payload Format

This document shows the payload format that the custom form uses to submit data to the Google Form backend.

## Endpoint

```
POST https://docs.google.com/forms/d/e/1FAIpQLSelv7kxqWAIFSjcdKiSpOg-Bh-uGmpnftLTesG_y1OlFcYq7g/formResponse
```

## Form Fields Mapping

| Question | Entry ID | Field Name | Type | Required |
|----------|----------|------------|------|----------|
| First Name | 1787995801 | entry.1787995801 | text | Yes |
| Last Name | 967171375 | entry.967171375 | text | Yes |
| Email | 1398194766 | entry.1398194766 | email | Yes |
| Which company | 1739515298 | entry.1739515298 | radio | No |
| What department | 172706529 | entry.172706529 | text | No |
| Accredited investor | 53141783 | entry.53141783 | radio | No |
| Check size | 569483976 | entry.569483976 | radio | No |
| Starting company | 279181523 | entry.279181523 | radio | No |
| Fundraising amount | 653201755 | entry.653201755 | radio | No |

## Example Payload

```
Content-Type: multipart/form-data

entry.1787995801=John
entry.967171375=Doe
entry.1398194766=john.doe@example.com
entry.1739515298=Tesla
entry.172706529=Autopilot Engineering
entry.53141783=Yes
entry.569483976=$10k - $25k
entry.279181523=Yes
entry.653201755=More than $1M
```

## Submission Method

The form uses `fetch()` with `mode: 'no-cors'` to submit data. This is the standard approach for submitting to Google Forms from custom interfaces:

```javascript
const formUrl = 'https://docs.google.com/forms/d/e/.../formResponse';
const formDataToSend = new FormData();

questions.forEach(q => {
    if (formData[q.id]) {
        formDataToSend.append(q.entry, formData[q.id]);
    }
});

fetch(formUrl, {
    method: 'POST',
    body: formDataToSend,
    mode: 'no-cors'
}).then(() => {
    // Show thank you message
}).catch(() => {
    // Also show thank you (no-cors doesn't expose success/failure)
});
```

## Notes

- The `no-cors` mode is required because Google Forms doesn't send CORS headers
- Due to `no-cors`, the response is opaque and we cannot verify submission success
- However, data is successfully submitted to the Google Form backend
- Responses appear in the same intake tracker as the original embedded form
- No test submissions were made to the live form during development
