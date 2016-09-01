```Java
import android.app.Activity;
import android.content.Intent;
import android.content.pm.PackageInfo;
import android.content.pm.PackageManager;
import android.graphics.BitmapFactory;
import android.net.Uri;
import android.os.Bundle;
import android.text.TextUtils;
import android.view.Gravity;
import android.view.LayoutInflater;
import android.view.View;
import android.widget.FrameLayout;
import android.widget.TextView;

import com.amap.api.location.AMapLocation;
import com.amap.api.location.AMapLocationClient;
import com.amap.api.location.AMapLocationClientOption;
import com.amap.api.location.AMapLocationListener;
import com.amap.api.maps.AMap;
import com.amap.api.maps.CameraUpdateFactory;
import com.amap.api.maps.LocationSource;
import com.amap.api.maps.MapView;
import com.amap.api.maps.model.BitmapDescriptorFactory;
import com.amap.api.maps.model.LatLng;
import com.amap.api.maps.model.Marker;
import com.amap.api.maps.model.MarkerOptions;
import com.amap.api.services.core.LatLonPoint;
import com.amap.api.services.geocoder.AoiItem;
import com.amap.api.services.geocoder.GeocodeResult;
import com.amap.api.services.geocoder.GeocodeSearch;
import com.amap.api.services.geocoder.RegeocodeQuery;
import com.amap.api.services.geocoder.RegeocodeResult;
import com.sdu.didi.openapi.DIOpenSDK;

import java.net.URISyntaxException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 * Created by wzl on 2016/6/13.
 */
public class MapActivity extends BaseActivity implements LocationSource, AMap.OnMarkerClickListener,
        AMap.InfoWindowAdapter, AMap.OnMapLoadedListener, GeocodeSearch.OnGeocodeSearchListener,
        AMapLocationListener {
    private MapView mapView;
    private AMap map;
    private AMapLocationClient mLocationClient = null;
    private AMapLocationClientOption mLocationOption = null;
    private static final String LATI = "latitude";
    private static final String LONGI = "longitude";
    private static final String HOSPITAL = "hospital";
    private static final String SHOW_DIDI = "show_didi";
    private String toLati = "";
    private String toLogi = "";
    private String fromLogi = "";
    private String fromLati = "";
    private Marker terminal;
    private View popView;
    private String hospital;
    private TextView hospitalTv;
    private TextView go;
    private boolean isShowDiDi;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_map);
        mapView = (MapView) findViewById(R.id.map);
        mapView.onCreate(savedInstanceState);
        init();
    }

    private void init() {
        isShowDiDi = getIntent().getBooleanExtra(SHOW_DIDI, false);
        if (isShowDiDi) {
            DIOpenSDK.registerApp(this, "didi496E6662616E317A31322B6C536363", "d35ecd7956e1598621dcb419d11453c7");
        }
        toLati = getIntent().getStringExtra(LATI);
        toLogi = getIntent().getStringExtra(LONGI);
        hospital = getIntent().getStringExtra(HOSPITAL);
        popView = LayoutInflater.from(this).inflate(R.layout.map_pop_view, null);
        hospitalTv = (TextView) popView.findViewById(R.id.title);
        hospitalTv.setText(TextUtils.isEmpty(hospital) ? "" : hospital);
        go = (TextView) popView.findViewById(R.id.go);
        go.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                openMap();
            }
        });
        if (isShowDiDi) {
            TextView didi = (TextView) popView.findViewById(R.id.didi);
            didi.setVisibility(View.VISIBLE);
            didi.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    startDiDi();
                }
            });
        }
        map = mapView.getMap();
        if (isShowDiDi) {
            TextView view = new TextView(this);
            view.setText("滴滴打车");
            view.setTextSize(AppUtils.dip2px(16) / getResources().getDisplayMetrics().scaledDensity + 0.5f);
            view.setTextColor(getResources().getColor(R.color.title_color));
            view.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    startDiDi();
                }
            });
            FrameLayout.LayoutParams params = new FrameLayout.LayoutParams(FrameLayout.LayoutParams.WRAP_CONTENT, FrameLayout.LayoutParams.WRAP_CONTENT);
            params.rightMargin = AppUtils.dip2px(60);
            params.topMargin = AppUtils.dip2px(15);
            params.gravity = Gravity.RIGHT;
            int padding = AppUtils.dip2px(8);
            view.setPadding(padding, padding, padding, padding);
            view.setLayoutParams(params);
            mapView.addView(view);
        }
        map.setOnMapLoadedListener(this);
        map.setOnMarkerClickListener(this);
        map.setInfoWindowAdapter(this);
        map.setLocationSource(this);
        map.setMyLocationEnabled(true);
        map.getUiSettings().setMyLocationButtonEnabled(true);
        map.getUiSettings().setCompassEnabled(true);
        map.getUiSettings().setScaleControlsEnabled(true);

        terminal = map.addMarker(new MarkerOptions()
                .icon(BitmapDescriptorFactory.fromBitmap(BitmapFactory
                        .decodeResource(getResources(), /*R.drawable.end*/R.drawable.location))).title(hospital).snippet("去这里"));
        terminal.setPosition(new LatLng(Double.parseDouble(toLati), Double.parseDouble(toLogi)));
        GeocodeSearch search = new GeocodeSearch(this);
        search.setOnGeocodeSearchListener(this);
        LatLonPoint latLonPoint = new LatLonPoint(Double.parseDouble(toLati), Double.parseDouble(toLogi));
        RegeocodeQuery query = new RegeocodeQuery(latLonPoint, 50,
                GeocodeSearch.AMAP);
        search.getFromLocationAsyn(query);
    }

    private void startDiDi() {
        HashMap<String, String> map = new HashMap<String, String>();
        map.put("fromlat", fromLati);
        map.put("fromlng", fromLogi);
        map.put("tolat", toLati);
        map.put("tolng", toLogi);
        map.put("biz", 1 + "");
        map.put("maptype", "soso");
        DIOpenSDK.showDDPage(this, map);
    }

    //优先调用高德app，其次百度app，如果两者均未安装则调用高德地图网页版
    private boolean openMap() {
        try {
            if (isAvailable("com.autonavi.minimap")) {
                Intent intent = Intent.getIntent("androidamap://viewMap?sourceApplication=" + AppUtils.getAppName() + "&poiname=" + hospital + "&lat=" + toLati + "&lon=" + toLogi + "&dev=0");
                startActivity(intent);
                return true;
            } else if (isAvailable("com.baidu.BaiduMap")) {
                Intent intent = Intent.getIntent("intent://map/marker?location=" + toLati + "," + toLogi + "&title=" + hospital + "&coord_type=gcj02&content=" + hospital + "&src=" + AppUtils.getAppName() + "#Intent;scheme=bdapp;package=com.baidu.BaiduMap;end");
                startActivity(intent); //启动调用
                return true;
            } else {
                String url = "http://m.amap.com/?q=" + toLati + "," + toLogi + "&name=" + hospital;
                Intent intent = new Intent(Intent.ACTION_VIEW);
                Uri content_url = Uri.parse(url);
                intent.setData(content_url);
                startActivity(intent);
                return true;
            }
        } catch (URISyntaxException e) {
            e.printStackTrace();
        }
        return false;
    }

    private boolean isAvailable(String packageName) {
        final PackageManager packageManager = getPackageManager();
        List<PackageInfo> pinfo = packageManager.getInstalledPackages(0);
        List<String> pName = new ArrayList<>();
        if (pinfo != null) {
            for (int i = 0; i < pinfo.size(); i++) {
                String pn = pinfo.get(i).packageName;
                pName.add(pn);
            }
        }
        return pName.contains(packageName);
    }

    private void initLoc() {
        mLocationClient = new AMapLocationClient(getApplicationContext());
        mLocationOption = new AMapLocationClientOption();
        mLocationClient.setLocationListener(this);
        mLocationOption.setLocationMode(AMapLocationClientOption.AMapLocationMode.Hight_Accuracy);
        mLocationOption.setNeedAddress(true);
        mLocationOption.setOnceLocation(false);
        mLocationOption.setWifiActiveScan(true);
        mLocationOption.setMockEnable(false);
        mLocationOption.setInterval(2000);
        mLocationClient.setLocationOption(mLocationOption);
        mLocationClient.startLocation();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        mapView.onDestroy();
    }

    @Override
    protected void onResume() {
        super.onResume();
        mapView.onResume();
    }

    @Override
    protected void onPause() {
        super.onPause();
        mapView.onPause();
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        mapView.onSaveInstanceState(outState);
    }

    private OnLocationChangedListener listener;

    @Override
    public void activate(OnLocationChangedListener listener) {
        this.listener = listener;
    }

    @Override
    public void deactivate() {
        this.listener = null;
        if (mLocationClient != null) {
            mLocationClient.stopLocation();
            mLocationClient.onDestroy();
        }
        mLocationClient = null;
    }

    public static void startActivity(Activity activity, String toLongitude, String toLatitude, String hospitalName) {
        Intent intent = new Intent(activity, MapActivity.class);
        intent.putExtra(LONGI, toLongitude);
        intent.putExtra(LATI, toLatitude);
        intent.putExtra(HOSPITAL, hospitalName);
        activity.startActivity(intent);
    }

    public static void startActivityWithDiDi(Activity activity, String toLongitude, String toLatitude, String hospitalName) {
        Intent intent = new Intent(activity, MapActivity.class);
        intent.putExtra(LONGI, toLongitude);
        intent.putExtra(LATI, toLatitude);
        intent.putExtra(HOSPITAL, hospitalName);
        intent.putExtra(SHOW_DIDI, true);
        activity.startActivity(intent);
    }

    @Override
    public boolean onMarkerClick(Marker marker) {
        return true;
    }

    @Override
    public View getInfoWindow(Marker marker) {
        return popView;
    }

    @Override
    public View getInfoContents(Marker marker) {
        return null;
    }

    @Override
    public void onMapLoaded() {
        initLoc();
        map.moveCamera(CameraUpdateFactory.changeLatLng(new LatLng(TextUtils.isEmpty(toLati) ? 21.190663d : Double.parseDouble(toLati), TextUtils.isEmpty(toLogi) ? 121.439024d : Double.valueOf(toLogi))));
        map.moveCamera(CameraUpdateFactory.zoomTo(17));
        terminal.showInfoWindow();
    }

    @Override
    public void onRegeocodeSearched(RegeocodeResult regeocodeResult, int i) {
        if (null == regeocodeResult || null == regeocodeResult.getRegeocodeAddress()) {
            return;
        }
        for (AoiItem item : regeocodeResult.getRegeocodeAddress().getAois()) {
            if (item.getAoiName().indexOf("医院") != -1) {
                TextView titleUi = ((TextView) popView.findViewById(R.id.title));
                titleUi.setText(item.getAoiName());
                break;
            }
        }
    }

    @Override
    public void onGeocodeSearched(GeocodeResult geocodeResult, int i) {
    }

    boolean flag = true;

    @Override
    public void onLocationChanged(AMapLocation aMapLocation) {
        if (listener != null && aMapLocation != null) {
            if (aMapLocation.getErrorCode() == 0) {
                fromLati = String.valueOf(aMapLocation.getLatitude());
                fromLogi = String.valueOf(aMapLocation.getLongitude());
                listener.onLocationChanged(aMapLocation);
                if (flag) {
                    flag = false;
                    map.moveCamera(CameraUpdateFactory.changeLatLng(new LatLng(TextUtils.isEmpty(toLati) ? 21.190663d : Double.parseDouble(toLati), TextUtils.isEmpty(toLogi) ? 121.439024d : Double.valueOf(toLogi))));
                    map.moveCamera(CameraUpdateFactory.zoomTo(17));
                }
            } else {
            }
        }
    }
}
```Java
