# ProviderTest

package kr.hkit.providertest02;

import android.content.ContentUris;
import android.content.ContentValues;
import android.net.Uri;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;


public class ProviderTest02Activity extends AppCompatActivity {

    EditText editName,editPhone;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        editName = (EditText) findViewById(R.id.editName);
        editPhone = (EditText) findViewById(R.id.editPhone);


    }

    //onClick method
    public void insert (View view) {
        ContentValues values = new ContentValues();

        String name = editName.getText().toString();
        String phone = editPhone.getText().toString();

        values.put(ContactsContract.RawContacts.ACCOUNT_TYPE,"com.google");
        values.put(ContactsContract.RawContacts.ACCOUNT_NAME, name);
        Uri rawContactUri = getContentResolver().insert(ContactsContract.RawContacts.CONTENT_URI, values);
        long rawContactId = ContentUris.parseId(rawContactUri);

        values = new ContentValues();
        values.put(ContactsContract.Data.RAW_CONTACT_ID, rawContactId);
        values.put(ContactsContract.Data.MIMETYPE, ContactsContract.CommonDataKinds.StructuredName.CONTENT_ITEM_TYPE);
        values.put(ContactsContract.CommonDataKinds.StructuredName.GIVEN_NAME, name);
        getContentResolver().insert(ContactsContract.Data.CONTENT_URI, values);

        values = new ContentValues();
        values.put(ContactsContract.Data.RAW_CONTACT_ID, rawContactId);
        values.put(ContactsContract.Data.MIMETYPE, ContactsContract.CommonDataKinds.Phone.CONTENT_ITEM_TYPE);
        values.put(ContactsContract.CommonDataKinds.Phone.NUMBER, phone);
        values.put(ContactsContract.CommonDataKinds.Phone.TYPE, ContactsContract.CommonDataKinds.Phone.TYPE_MOBILE);
        getContentResolver().insert(ContactsContract.Data.CONTENT_URI, values);

        Toast.makeText(getApplicationContext(),name + "님이 연락처에 추가되었습니다.",Toast.LENGTH_SHORT).show();
    }

    //update (change phone from name)
    public void update (View view) {
        ContentValues values = new ContentValues();

        String name = editName.getText().toString();

        Toast.makeText(getApplicationContext(),name + "님의 연락처가 수정되었습니다.",Toast.LENGTH_SHORT).show();
    }

    //delete (delete from name)
    public void delete (View view) {
        ContentValues values = new ContentValues();

        String name = editName.getText().toString();

        Uri rawContactUri = getContentResolver().insert(ContactsContract.RawContacts.CONTENT_URI, values);
        long rawContactId = ContentUris.parseId(rawContactUri);

        values.put(ContactsContract.Data.RAW_CONTACT_ID, rawContactId);
        values.put(ContactsContract.Data.MIMETYPE, ContactsContract.CommonDataKinds.Phone.CONTENT_ITEM_TYPE);
        values.remove(ContactsContract.RawContacts.ACCOUNT_NAME);
    }


    //search (search phone from name)
    public void query (View view) {
        String name = editName.getText().toString();

    }
}
