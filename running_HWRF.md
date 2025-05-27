# HWRF Running on HPC

```console
$ cd $HOME
$ mkdir hwrfrun
$ cd hwrfrun/
$ export user=tr10
$ ln -sf /home/${user}/HWRFdev/run/* .
$ rm namelist.input*
$ cp /home/${user}/HWRFdev/run/namelist.input .
$ ln -sf /home/${user}/HWPSdev/*exe .
$ ln -sf /home/${user}/HWPSdev/link_grib.csh .
$ ln -sf /home/${user}/HWPSdev/geogrid .
$ ln -sf /home/${user}/HWPSdev/ungrib .
$ ln -sf /home/${user}/HWPSdev/metgrid .
$ cp /home/${user}/HWPSdev/namelist.wps .
$ ln -sf /home/${user}/HWPSdev/ungrib/Variable_Tables/Vtable.GFS Vtable
$ cd geogrid
$ ln -sf GEOGRID.TBL.NMM GEOGRID.TBL
$ cd ../metgrid
$ ln -sf METGRID.TBL.NMM METGRID.TBL
$ cd ..
$ ./link_grib.csh /mnt-storage1/userk01/LATIHAN/GDAS/gdas1.fnl0p25.202104*
$ vi namelist.wps
```

```console
&share
 wrf_core = 'NMM',
 max_dom = 3,
 start_date = '2021-04-04_00:00:00','2021-04-04_00:00:00','2021-04-04_00:00:00',
 end_date   = '2021-04-13_00:00:00','2021-04-13_00:00:00','2021-04-13_00:00:00',
 interval_seconds = 21600
 io_form_geogrid = 2,
/

&geogrid
 parent_id         =   1,   1,   2,
 parent_grid_ratio =   1,  3,  3,
 j_parent_start    =   1,  130,   40,
 e_we              =  300, 124,   154,
 e_sn              =  300, 124,   154,
 geog_data_res = '30s+default','30s+default','30s+default'
 dx = 0.07500,
 dy = 0.07500,
 map_proj = 'rotated_ll',
 ref_lat   =  -10.0000,
 ref_lon   =  123.0000,
 geog_data_path = '/mnt-storage1/userk01/LATIHAN/GEOG/'
/

&ungrib
 out_format = 'WPS',
 prefix = 'FILE',
/

&metgrid
 fg_name = 'FILE'
 io_form_metgrid = 2, 
/
```

```console
$ ./geogrid.exe
$ ./ungrib.exe
$ ./metgrid.exe
$ nano namelist.input

```

```console
 &time_control
 run_days                            = 9,
 run_hours                           = 0,   
 run_minutes                         = 0,
 run_seconds                         = 0,
 start_year                          = 2021,     2021, 2021, 
 start_month                         = 04,       04, 04,
 start_day                           = 04,       04, 04,
 start_hour                          = 00,       00, 00,
 start_minute                        = 00,       00, 00,
 start_second                        = 00,       00, 00,
 tstart                              = 00,       
 end_year                            = 2021,     2021, 2021,
 end_month                           = 04,       04, 04,
 end_day                             = 13,       13, 13,
 end_hour                            = 00,       00, 00,
 end_minute                          = 00,       00, 00,
 end_second                          = 00,       00, 00,
 interval_seconds                    = 21600,
 history_interval                    = 60,       60, 60,
 frames_per_outfile                  = 1,        1, 60,
 restart                             = .false.,
 restart_interval                    = 5400,
 reset_simulation_start              = F,
 io_form_input                       = 2
 io_form_history                     = 2
 io_form_restart                     = 2
 io_form_boundary                    = 2
 io_form_auxinput1                   = 2
 debug_level                         = 1 
 /

 &domains
 time_step                           = 27,
 time_step_fract_num                 = 0,
 time_step_fract_den                 = 1,
 max_dom                             = 3,
 e_we                                = 300, 124,   154,
 e_sn                                = 300, 124,   154,
 e_vert                              = 38,       38, 38,
 num_metgrid_levels                  = 34,
 dx                                  = .075,     .025,	.008333,
 dy                                  = .075,     .025,	.008333,
 p_top_requested                     = 5000. 
 ptsgm                               = 42000.,
 grid_id                             = 1,        2,	3,
 parent_id                           = 0,        1,     2,  
 i_parent_start                      = 1,        130,   40,
 j_parent_start                      = 1,        130,   40,
 parent_grid_ratio                   = 1,        3, 	3,
 parent_time_step_ratio              = 1,        3, 	3,
 /

 &physics
 mp_physics                          = 5,        5,	5,
 ra_lw_physics                       = 99,       99,	99,		
 ra_sw_physics                       = 99,       99,	99,		
 nrads                               = 105,      315,	945,
 nradl                               = 105,      315,	945,
 co2tf                               = 1,
 sf_sfclay_physics                   = 2,        2,	2,
 sf_surface_physics                  = 2,        2, 	2,
 bl_pbl_physics                      = 2,        2,	2,
 nphs                                = 6,        18,	54,
 cu_physics                          = 2,        2,	2,
 ncnvc                               = 6,        18,	54,
 tprec                               = 3,        3,	3,
 theat                               = 6,        6,	6,
 tclod                               = 6,        6,	6,
 trdsw                               = 6,        6,	6,
 trdlw                               = 6,        6,	6,
 tsrfc                               = 6,        6,	6,
 pcpflg                              = .false.,  .false., .false.,
 num_soil_layers                     = 4,
 mp_zero_out                         = 0
 gwd_opt                             = 0
 /

 &dynamics
 coac                                = 1.6,
 codamp                              = 6.4,
 slophc                              = 0.0064,
 euler_adv                           = .true.,
 idtadt                              = 2,
 idtadc                              = 1
 /

 &bdy_control
 spec_bdy_width                      = 1,
 specified                           = .true.,
 nested                              = .false.
 /

 &fdda
 /
 
 &grib2
 /

 &namelist_quilt
 nio_tasks_per_group = 0,
 nio_groups = 1
 /
```

## Running WRF process
```console
$ ./real_nmm.exe
$ mpirun -n 2 ./wrf.exe &
$ tail -f rsl.out.0000

```

## Check the results
Check the header of entire file
```console
$ ncdump -h wrfout_d01_2021-04-04_00:00:00
```
Check the visualization of entire file
```console
$ ncview wrfout_d01_2021-04-04_00:00:00
```
Example of another visualization using python script
```console
import netCDF4 as nc
import cartopy.crs as ccrs
import matplotlib.pyplot as plt

fh=nc.Dataset('hwrfrun/wrfout_d01_2021-04-03_06:00:00')
lat=fh.variables['VLAT'][0]
lon=fh.variables['VLON'][0]
mslp=fh.variables['MSLP'][0]/100

ax = plt.axes(projection=ccrs.PlateCarree())
ax.coastlines()
ax.set_extent([lon[0,0], lon[0,-1], lat[0,0], lat[-1,0]])
ax.gridlines(draw_labels=True,color='black',alpha=0.5,linestyle='--')
cs=ax.contourf(lon,lat,mslp,cmap= 'Blues_r', transform=ccrs.PlateCarree())
plt.colorbar(cs,orientation='horizontal')
plt.show()
```
## Finish
