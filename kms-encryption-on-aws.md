# KMS & Encryption on AWS

### AWS KMS 101
- Exam Tips
  - The Customer Master Key:
    - CMK
      - alias
      - creation date
      - description
      - key state
      - key material (either customer provided or AWS provided)
    - Can NEVER be exported  
    - Key material options:
      - User KMS generated key material
      - Your own key material

### KMS API Calls
- Exam Tips
  - aws kms encrypt
  - aws kms decrypt
  - aws kms re-encrypt
  - aws kms enable-key-rotation

### KMS Envelope Encryption
- Exam Tips
  - The customer Master Key:
    - Customer Master Key used to decrypt the data key (envelope key)
    - Envelope Key is used to decrypt the data


### AWS Key Management Service
- Exam Tips
  - Setup a Customer Master Key:
    - Create Alias and Description
    - Choose material option
    - Define Key Administrative Permissions
      - IAM users/roles that can administer (but not use) the key through the KMS API
    - Define Key Usage Permissions
      - IAM users/roles that can use the key to encrypt and decrypt data
    - Key material options:
      - Use KMS generated key material
      - Your own key material
