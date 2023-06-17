---
layout: default
title: FUS
description: FUS Protocol
nav_order: 2
---

## Warning
If you just wanna to download Samsung's firmware from their official servers, click Syndical button below. \
It is an implementation of this protocol, tested by TheAirBlow.

<h2>Authentication <p style="font-size: 14px !important;" class="label label-yellow">Old knowledge</p></h2>
Headers:
```
Authorization: FUS nonce="encrypted_nonce", signature="nonce_to_token", nc="", type="", realm="", newauth="1"
User-Agent: Kies2.0_FUS
Cache-Control: no-cache
```
### Keys
Key1: `hqzdurufm2c8mf6bsjezu1qgveouv7c7` \
Key2: `w13r4cvf4hctaujv`
### Decrypt nonce
Method: `AES CBC PKCS7 DECRYPT` \
Input: Base64 decoded nonce \
Key: Key1
### Convert nonce to key
For each first 16 characters in decrypted nonce:
* Convert char to UTF-32 position
* Get the remainder of division by 16
* Use it as an offset to Key1 and add it to key

Add Key2 to the end of key.
### Convert nonce to token
Method: `AES CBC PKCS7 ENCRYPT` \
Input: Nonce \
Key: Nonce converted to key
### Logic check
We have two inputs. \
For each character in Input 2:
* Convert char to UTF-32 position
* Do logical and with 0xF on it
* Use it as an offset into Input 1 and add it to output

## Decryption of firmware
Method: `AES ECB PKCS7 DECRYPT`
### Version 2
The key is just MD5 hash of `region_code:model:firmware_version`
### Version 4
* Get Logic Check output:
```
Input 1: LATEST_FW_VERSION data value
Input 2: LOGIC_VALUE_FACTORY data value for BINARY_NATURE = 1, LOGIC_VALUE_HOME for BINARY_NATURE = 0
```
* Get it's MD5 hashsum

<h2>Endpoints <p style="font-size: 14px !important;" class="label label-yellow">Old knowledge</p></h2>
### http://cloud-neofussvr.sslcs.cdngc.net/
#### NF_DownloadBinaryForMass.do
Query paramaters: `?file=<path><filename>`. \
No request body is required. \
Method: GET \
Response is the encrypted firmware.
### https://neofussvr.sslcs.cdngc.net/
#### NF_DownloadGenerateNonce.do
No request body is required. \
Method: POST \
Response body:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<FUSMsg>
	<FUSHdr>
		<ProtoVer>1.0</ProtoVer>
		<SessionID></SessionID>
		<MsgID>1</MsgID>
	</FUSHdr>
	<FUSBody>
		<Results>
			<CmdRef>2</CmdRef>
			<Status>200</Status>
		</Results>
	</FUSBody>
</FUSMsg>
```
#### NF_DownloadBinaryInform.do
`CLIENT_PRODUCT` can be absolutely anything. \
LOGIC_CHECK inputs:
```
Input 1: Firmware version
Input 2: Decrypted nonce
```
Method: POST \
Request body:
```xml
<FUSMsg>
    <FUSHdr>
        <ProtoVer>1.0</ProtoVer>
    </FUSHdr>
    <FUSBody>
        <Put>
            <ACCESS_MODE>
                <Data>2</Data>
            </ACCESS_MODE>
            <CLIENT_PRODUCT>
                <Data>Smart Switch</Data>
            </CLIENT_PRODUCT>
            <BINARY_NATURE>
                <Data>1</Data>
            </BINARY_NATURE>
            <DEVICE_FW_VERSION>
                <Data>A207FXXU2CUI2/A207FOXM2CUI2/A207FXXU2CUI2/A207FXXU2CUI2</Data>
            </DEVICE_FW_VERSION>
            <DEVICE_LOCAL_CODE>
                <Data>SER</Data>
            </DEVICE_LOCAL_CODE>
            <DEVICE_MODEL_NAME>
                <Data>SM-A207F</Data>
            </DEVICE_MODEL_NAME>
            <LOGIC_CHECK>
                <Data>2FC/2C2A0AC0202X</Data>
            </LOGIC_CHECK>
        </Put>
    </FUSBody>
</FUSMsg>
```
Response body:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<FUSMsg>
	<FUSHdr>
		<ProtoVer>1.0</ProtoVer>
		<SessionID></SessionID>
		<MsgID>2</MsgID>
	</FUSHdr>
	<FUSBody>
		<Results>
			<CmdRef>2</CmdRef>
			<Status>200</Status>
			<LATEST_FW_VERSION>
				<Data>A207FXXU2CUI2/A207FOXM2CUI2/A207FXXU2CUI2/A207FXXU2CUI2</Data>
			</LATEST_FW_VERSION>
		</Results>
		<Put>
			<CmdID>1</CmdID>
			<BINARY_CRC>
				<Data>1830059332</Data>
			</BINARY_CRC>
			<BINARY_NAME>
				<Data>SM-A207F_2_20210928113735_g3wsdfg48a_fac.zip.enc4</Data>
			</BINARY_NAME>
			<BINARY_SIZE>
				<Data>4449288</Data>
			</BINARY_SIZE>
			<BINARY_BYTE_SIZE>
				<Data>4556071616</Data>
			</BINARY_BYTE_SIZE>
			<DESCRIPTION>
				<Data>https://doc.samsungmobile.com/SM-A207F/SER/doc.html</Data>
			</DESCRIPTION>
			<DESCRIPTION_FLAG>
				<Data>1</Data>
			</DESCRIPTION_FLAG>
			<SUPPORT_HIDDEN>
				<Data>0</Data>
			</SUPPORT_HIDDEN>
			<FW_INDEX>
				<Data>2</Data>
			</FW_INDEX>
			<DEVICE_MODEL_NAME>
				<Data>SM-A207F</Data>
			</DEVICE_MODEL_NAME>
			<DEVICE_MODEL_TYPE>
				<Data>9</Data>
			</DEVICE_MODEL_TYPE>
			<MODEL_PATH>
				<Data>/neofus/910/</Data>
			</MODEL_PATH>
			<DEVICE_BUYER_CODE>
				<Data>XX</Data>
			</DEVICE_BUYER_CODE>
			<DEVICE_LOCAL_CODE>
				<Data>SER</Data>
			</DEVICE_LOCAL_CODE>
			<DEVICE_AID_CODE>
				<Data></Data>
			</DEVICE_AID_CODE>
			<DEVICE_CC_CODE>
				<Data></Data>
			</DEVICE_CC_CODE>
			<ANNOUNCE_FLAG>
				<Data>0</Data>
			</ANNOUNCE_FLAG>
			<ANNOUNCE>
				<Data></Data>
			</ANNOUNCE>
			<NOTIFY>
				<Data>1</Data>
			</NOTIFY>
			<LAST_MODIFIED>
				<Data>20211007165909</Data>
			</LAST_MODIFIED>
			<DEVICE_PLATFORM>
				<Data>Android</Data>
			</DEVICE_PLATFORM>
			<PLATFORM_SUPPORT>
				<Data>0</Data>
			</PLATFORM_SUPPORT>
			<DEVICE_BOOT_FILE>
				<Data>BL_A207FXXU2CUI2_CL22251137_QB43560309_REV00_user_low_ship_MULTI_CERT.tar.md5</Data>
			</DEVICE_BOOT_FILE>
			<DEVICE_PDA_CODE1_FILE>
				<Data>AP_A207FXXU2CUI2_CL22251137_QB43560309_REV00_user_low_ship_MULTI_CERT_meta_RKEY_OS11.tar.md5</Data>
			</DEVICE_PDA_CODE1_FILE>
			<DEVICE_CSC_CODE2_FILE>
				<Data></Data>
			</DEVICE_CSC_CODE2_FILE>
			<DEVICE_PHONE_FONT_FILE>
				<Data>CP_A207FXXU2CUI2_CP20395837_CL22251137_QB43560309_REV00_user_low_ship_MULTI_CERT.tar.md5</Data>
			</DEVICE_PHONE_FONT_FILE>
			<DEVICE_CONTENTS_DATA_FILE>
				<Data></Data>
			</DEVICE_CONTENTS_DATA_FILE>
			<DEVICE_LANGUAGE_FILE>
				<Data></Data>
			</DEVICE_LANGUAGE_FILE>
			<DEVICE_BINARY_FOLDER>
				<Data></Data>
			</DEVICE_BINARY_FOLDER>
			<DEVICE_AMSS_FILE>
				<Data></Data>
			</DEVICE_AMSS_FILE>
			<DEVICE_RSRC1_FILE>
				<Data></Data>
			</DEVICE_RSRC1_FILE>
			<DEVICE_RSRC2_FILE>
				<Data></Data>
			</DEVICE_RSRC2_FILE>
			<DEVICE_CSC_FILE>
				<Data>CSC_OMC_OXM_A207FOXM2CUI2_CL22251137_QB43560309_REV00_user_low_ship_MULTI_CERT.tar.md5</Data>
			</DEVICE_CSC_FILE>
			<DEVICE_X_FS_FILE>
				<Data></Data>
			</DEVICE_X_FS_FILE>
			<DEVICE_APPS_FILE>
				<Data></Data>
			</DEVICE_APPS_FILE>
			<DEVICE_SHPAPP_FILE>
				<Data></Data>
			</DEVICE_SHPAPP_FILE>
			<DEVICE_FOTA_FILE>
				<Data></Data>
			</DEVICE_FOTA_FILE>
			<DEVICE_PFS_FILE>
				<Data></Data>
			</DEVICE_PFS_FILE>
			<DEVICE_PFS_SIZE>
				<Data></Data>
 			</DEVICE_PFS_SIZE>
			<DEVICE_BSY_FILE>
				<Data></Data>
			</DEVICE_BSY_FILE>
			<DEVICE_CDS_FILE>
				<Data></Data>
			</DEVICE_CDS_FILE>
			<DEVICE_UIIMAGE_FILE>
				<Data></Data>
			</DEVICE_UIIMAGE_FILE>
			<DEVICE_PSIFLASH_FILE>
				<Data></Data>
			</DEVICE_PSIFLASH_FILE>
			<DEVICE_INI_FILE>
				<Data></Data>
			</DEVICE_INI_FILE>
			<DEVICE_DSP1_FILE>
				<DATA></DATA>
			</DEVICE_DSP1_FILE>
			<DEVICE_DSP2_FILE>
				<DATA></DATA>
			</DEVICE_DSP2_FILE>
			<DEVICE_EFS_FILE>
				<DATA></DATA>
			</DEVICE_EFS_FILE>
			<DEVICE_BOOT_ADDRESS>
				<Data></Data>
			</DEVICE_BOOT_ADDRESS>
			<DEVICE_CODE1_ADDRESS>
				<Data></Data>
			</DEVICE_CODE1_ADDRESS>
 			<DEVICE_CODE2_ADDRESS>
				<Data></Data>
			</DEVICE_CODE2_ADDRESS>
			<DEVICE_FONT_ADDRESS>
				<Data></Data>
			</DEVICE_FONT_ADDRESS>
			<DEVICE_DATA_ADDRESS>
				<Data></Data>
			</DEVICE_DATA_ADDRESS>
			<DEVICE_VIA_BOOT_FILE>
				<Data></Data>
			</DEVICE_VIA_BOOT_FILE>
			<DEVICE_VIA_CODE_FILE>
				<Data></Data>
			</DEVICE_VIA_CODE_FILE>
			<DEVICE_PIT_FILE>
				<Data></Data>
			</DEVICE_PIT_FILE>
			<CURRENT_DISPLAY_VERSION>
				<Data>A207FXXU2CUI2/A207FOXM2CUI2/A207FXXU2CUI2/A207FXXU2CUI2</Data>
			</CURRENT_DISPLAY_VERSION>
			<LATEST_DISPLAY_VERSION>
				<Data>A207FXXU2CUI2/A207FOXM2CUI2/A207FXXU2CUI2/A207FXXU2CUI2</Data>
			</LATEST_DISPLAY_VERSION>
			<CASH_SERVER_IP>
				<Data></Data>
			</CASH_SERVER_IP>
			<BLOCKING_APP>
				<Data>0</Data>
			</BLOCKING_APP>
			<BINARY_EMERGENCY_OTP_RECEIVE>
				<Data></Data>
			</BINARY_EMERGENCY_OTP_RECEIVE>
			<DEVICE_IMEI_NUMBER>
				<Data></Data>
			</DEVICE_IMEI_NUMBER>
			<UPDATE_NOTICE>
				<Data>0</Data>
			</UPDATE_NOTICE>
			<BINARY_TYPE>
				<Data>0</Data>
			</BINARY_TYPE>
			<SUPPORT_BD_20>
				<Data>0</Data>
			</SUPPORT_BD_20>
			<BINARY_CRC_BD_20>
				<Data></Data>
			</BINARY_CRC_BD_20>
			<BINARY_SIZE_BD_20>
				<Data></Data>
			</BINARY_SIZE_BD_20>
			<BINARY_NAME_BD_20>
				<Data></Data>
			</BINARY_NAME_BD_20>
			<LAST_MODIFIED_BD_20>
				<Data></Data>
			</LAST_MODIFIED_BD_20>
			<DEVICE_BINARY_FOLDER_BD_20>
				<Data></Data>
			</DEVICE_BINARY_FOLDER_BD_20>
			<DEVICE_AMSS_FILE_BD_20>
				<Data></Data>
			</DEVICE_AMSS_FILE_BD_20>
			<DEVICE_RSRC1_FILE_BD_20>
				<Data></Data>
			</DEVICE_RSRC1_FILE_BD_20>
			<DEVICE_RSRC2_FILE_BD_20>
				<Data></Data>
			</DEVICE_RSRC2_FILE_BD_20>
			<DEVICE_X_FS_FILE_BD_20>
				<Data></Data>
			</DEVICE_X_FS_FILE_BD_20>
			<DEVICE_APPS_FILE_BD_20>
				<Data></Data>
			</DEVICE_APPS_FILE_BD_20>
			<DEVICE_CSC_FILE_BD_20>
				<Data></Data>
			</DEVICE_CSC_FILE_BD_20>
			<DEVICE_SHPAPP_FILE_BD_20>
				<Data></Data>
			</DEVICE_SHPAPP_FILE_BD_20>
			<DEVICE_FOTA_FILE_BD_20>
				<Data></Data>
			</DEVICE_FOTA_FILE_BD_20>
			<DEVICE_PFS_FILE_BD_20>
				<Data></Data>
			</DEVICE_PFS_FILE_BD_20>
			<DEVICE_PFS_SIZE_BD_20>
				<Data></Data>
			</DEVICE_PFS_SIZE_BD_20>
			<FW_INDEX_BD_20>
				<Data></Data>
			</FW_INDEX_BD_20>
			<LATEST_DISPLAY_VERSION_BD_20>
				<Data></Data>
			</LATEST_DISPLAY_VERSION_BD_20>
			<LATEST_FW_VERSION_BD_20>
				<Data></Data>
			</LATEST_FW_VERSION_BD_20>
			<BINARY_EMERGENCY_OTP_RECEIVE_BD_20>
				<Data></Data>
			</BINARY_EMERGENCY_OTP_RECEIVE_BD_20>
			<BNR_SUPPORT>
				<Data>0</Data>
			</BNR_SUPPORT>
			<CDNURL>
				<Data>https://neofussvr.sslcs.cdngc.net/</Data>
			</CDNURL>
			<DEVICE_MODEL_DISPLAYNAME>
				<Data>SSP</Data>
			</DEVICE_MODEL_DISPLAYNAME>
			<FACTORY_SUPPORT>
				<Data>3</Data>
			</FACTORY_SUPPORT>
			<FACTORY_DO_EXIST>
				<Data>1</Data>
			</FACTORY_DO_EXIST>
			<BINARY_NATURE>
				<Data>1</Data>
			</BINARY_NATURE>
			<FACTORY_KEY_TYPE>
				<Data>21</Data>
			</FACTORY_KEY_TYPE>
			<NOTICE_URL_FIRST>
				<Data></Data>
			</NOTICE_URL_FIRST>
			<NOTICE_URL_LAST>
				<Data></Data>
			</NOTICE_URL_LAST>
			<MEMORY_SIZE_CHECK>
				<Data>3</Data>
			</MEMORY_SIZE_CHECK>
			<ROUTING_SUPPORT>
				<Data>0</Data>
			</ROUTING_SUPPORT>
			<SN_TYPE>
				<Data></Data>
			</SN_TYPE>
			<SETTING_INFO>
				<Data></Data>
			</SETTING_INFO>
			<MEMORY_ANNOUNCE>
				<Data></Data>
			</MEMORY_ANNOUNCE>
			<UPGRADE_VARIABLE>
				<Data>0</Data>
			</UPGRADE_VARIABLE>
			<BUTTON_TYPE>
				<Data>0</Data>
			</BUTTON_TYPE>
			<ADD_DESCRIPTION_FLAG>
				<Data>1</Data>
			</ADD_DESCRIPTION_FLAG>
			<ADD_DESCRIPTION>
				<Data>https://doc.samsungmobile.com/SM-A207F/SER/doc.html</Data>
			</ADD_DESCRIPTION>
			<ADD_ANNOUNCE_FLAG>
				<Data>0</Data>
			</ADD_ANNOUNCE_FLAG>
			<ADD_ANNOUNCE>
				<Data></Data>
			</ADD_ANNOUNCE>
			<ADD_LATEST_DISPLAY_VERSION>
				<Data>A207FXXU2CUI2/A207FOXM2CUI2/A207FXXU2CUI2/A207FXXU2CUI2</Data>
			</ADD_LATEST_DISPLAY_VERSION>
			<ADD_LATEST_FW_VERSION>
				<Data>A207FXXU2CUI2/A207FOXM2CUI2/A207FXXU2CUI2/A207FXXU2CUI2</Data>
			</ADD_LATEST_FW_VERSION>
			<CURRENT_OS_VERSION>
				<Data>R(Android 11)</Data>
			</CURRENT_OS_VERSION>
			<LATEST_OS_VERSION>
				<Data>R(Android 11)</Data>
			</LATEST_OS_VERSION>
			<ADD_OS_VERSION>
				<Data>R(Android 11)</Data>
			</ADD_OS_VERSION>
			<ADD_NOTICE_URL_FIRST>
				<Data></Data>
			</ADD_NOTICE_URL_FIRST>
			<ADD_NOTICE_URL_LAST>
				<Data></Data>
			</ADD_NOTICE_URL_LAST>
 			<OBEX_SUPPORT>
				<Data>0</Data>
			</OBEX_SUPPORT>
			<ABSOLUTE_SUPPORT>
				<Data>0</Data>
			</ABSOLUTE_SUPPORT>
			<COMMON_PLUGIN>
				<Data>1</Data>
			</COMMON_PLUGIN>
			<VERSION_HELP_TEXT>
				<Data>1</Data>
			</VERSION_HELP_TEXT>
			<LOGIC_OPTION_HOME>
				<Data>1</Data>
			</LOGIC_OPTION_HOME>
			<LOGIC_VALUE_HOME>
                                <Data>ehbkrulq43h44mqw</Data>
			</LOGIC_VALUE_HOME>
			<LOGIC_OPTION_FACTORY>
				<Data>1</Data>
			</LOGIC_OPTION_FACTORY>
			<LOGIC_VALUE_FACTORY>
				<Data>ehbkrulq43h44mqw</Data>
			</LOGIC_VALUE_FACTORY>
			<CDN_TRAFFIC>
				<Data></Data>
			</CDN_TRAFFIC>
			<CDN_TRAFFIC_OPTION>
				<Data></Data>
			</CDN_TRAFFIC_OPTION>
			<BATTERY_STANDARDS>
				<Data>3700</Data>
			</BATTERY_STANDARDS>
			<BATTERY_PERCENT>
				<Data>20</Data>
			</BATTERY_PERCENT>
			<SHARING_BINARY>
				<Data>1</Data>
			</SHARING_BINARY>
			<DEVICE_CSC_HOME_FILE>
				<Data>HOME_CSC_OMC_OXM_A207FOXM2CUI2_CL22251137_QB43560309_REV00_user_low_ship_MULTI_CERT.tar.md5</Data>
			</DEVICE_CSC_HOME_FILE>

			<USER_DATA_BINARY>
				<Data>0</Data>
			</USER_DATA_BINARY>
			<DEVICE_USER_DATA_FILE>
				<Data></Data>
			</DEVICE_USER_DATA_FILE>
 			<DVIF_SALES_VER>
				<Data>1</Data>
			</DVIF_SALES_VER>
			<SIZE_CHECK_PATH>
				<Data>0</Data>
			</SIZE_CHECK_PATH>
			<SSP_DEVICE_SIZECHECK>
				<Data>1</Data>
			</SSP_DEVICE_SIZECHECK>
		</Put>
	</FUSBody>
</FUSMsg>
```
#### NF_DownloadBinaryInitForMass.do
LOGIC_CHECK inputs:
```
Input 1: First 16 characters of filename without extension
Input 2: Decrypted nonce
```
Method: POST \
Request body:
```xml
<FUSMsg>
    <FUSHdr>
        <ProtoVer>1.0</ProtoVer>
    </FUSHdr>
    <FUSBody>
        <Put>
            <BINARY_FILE_NAME>
                <Data>SM-A207F_2_20210928113735_g3wsdfg48a_fac.zip.enc4</Data>
            </BINARY_FILE_NAME>
            <LOGIC_CHECK>
                <Data>agf3_3wf3wdsgdsf</Data>
            </LOGIC_CHECK>
        </Put>
    </FUSBody>
</FUSMsg>
```
Response body:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<FUSMsg>
	<FUSHdr>
		<ProtoVer>1.0</ProtoVer>
		<SessionID></SessionID>
		<MsgID>1</MsgID>
	</FUSHdr>
	<FUSBody>
		<Results>
			<CmdRef>2</CmdRef>
			<Status>200</Status>
		</Results>
	</FUSBody>
</FUSMsg>
```
